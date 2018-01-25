---
title: "Auxiliares de marcação em formulários do ASP.NET Core"
author: rick-anderson
description: "Descreve interno de auxiliares de marcação usados com formulários."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fd51755e1dc9a1dfb9ab5cc4558f7da9475ce32
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="df9f9-103">Introdução ao uso de auxiliares de marcação em formulários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="df9f9-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="df9f9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), e [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="df9f9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="df9f9-105">Este documento demonstra trabalhar com formulários e os elementos HTML comumente usados em um formulário.</span><span class="sxs-lookup"><span data-stu-id="df9f9-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="df9f9-106">O HTML [formulário](https://www.w3.org/TR/html401/interact/forms.html) elemento fornece o uso de aplicativos web mecanismo principal para enviar dados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="df9f9-107">A maioria deste documento descreve [auxiliares de marcação](tag-helpers/intro.md) e como eles podem ajudar você a criar produtiva robustos formulários HTML.</span><span class="sxs-lookup"><span data-stu-id="df9f9-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="df9f9-108">É recomendável que você leia [Introdução ao auxiliares de marcação](tag-helpers/intro.md) antes de ler este documento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="df9f9-109">Em muitos casos, auxiliares HTML fornecem uma abordagem alternativa para um auxiliar de marca específica, mas é importante reconhecer que auxiliares de marcação não substitua auxiliares HTML e não é um auxiliar de marca para cada auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="df9f9-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="df9f9-110">Quando existe uma alternativa de auxiliar HTML, ele é mencionado.</span><span class="sxs-lookup"><span data-stu-id="df9f9-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="df9f9-111">O auxiliar de marca de formulário</span><span class="sxs-lookup"><span data-stu-id="df9f9-111">The Form Tag Helper</span></span>

<span data-ttu-id="df9f9-112">O [formulário](https://www.w3.org/TR/html401/interact/forms.html) auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="df9f9-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="df9f9-113">Gera o HTML [ \<formulário >](https://www.w3.org/TR/html401/interact/forms.html) `action` valor de atributo para uma ação do controlador MVC ou uma rota nomeada</span><span class="sxs-lookup"><span data-stu-id="df9f9-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="df9f9-114">Gera um oculta [solicitação de Token de verificação](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="df9f9-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="df9f9-115">Fornece o `asp-route-<Parameter Name>` atributo, onde `<Parameter Name>` é adicionado aos valores de rota.</span><span class="sxs-lookup"><span data-stu-id="df9f9-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="df9f9-116">O `routeValues` parâmetros para `Html.BeginForm` e `Html.BeginRouteForm` fornecem funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="df9f9-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="df9f9-117">Tem uma alternativa de auxiliar HTML `Html.BeginForm` e`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="df9f9-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="df9f9-118">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="df9f9-119">O auxiliar de marca de formulário acima gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="df9f9-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="df9f9-120">O tempo de execução do MVC gera o `action` valor do atributo dos atributos do auxiliar de marca de formulário `asp-controller` e `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="df9f9-121">O auxiliar de marca de formulário também gera oculto [solicitação de Token de verificação](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="df9f9-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="df9f9-122">Proteger um formulário HTML puro contra falsificação de solicitação entre sites é difícil, o auxiliar de marca de formulário fornece este serviço para você.</span><span class="sxs-lookup"><span data-stu-id="df9f9-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="df9f9-123">Usando uma rota nomeada</span><span class="sxs-lookup"><span data-stu-id="df9f9-123">Using a named route</span></span>

<span data-ttu-id="df9f9-124">O `asp-route` atributo do auxiliar de marca também pode gerar a marcação para o HTML `action` atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="df9f9-125">Um aplicativo com um [rota](../../fundamentals/routing.md) chamado `register` poderia usar a seguinte marcação para a página de registro:</span><span class="sxs-lookup"><span data-stu-id="df9f9-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="df9f9-126">Muitas das exibições no *modos de exibição/conta* pasta (gerado quando você cria um novo aplicativo web com *contas de usuário individuais*) contêm o [asp de rota de returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atributo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="df9f9-127">Com os modelos internos, `returnUrl` só é preenchida automaticamente quando você tentar acessar um recurso autorizado, mas não é autenticado ou autorizado.</span><span class="sxs-lookup"><span data-stu-id="df9f9-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="df9f9-128">Quando você tenta um acesso não autorizado, o middleware de segurança redireciona para a página de logon com o `returnUrl` definido.</span><span class="sxs-lookup"><span data-stu-id="df9f9-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="df9f9-129">O auxiliar de marca de entrada</span><span class="sxs-lookup"><span data-stu-id="df9f9-129">The Input Tag Helper</span></span>

<span data-ttu-id="df9f9-130">O auxiliar de marca de entrada associa um HTML [ \<entrada >](https://www.w3.org/wiki/HTML/Elements/input) elemento para uma expressão de modelo no modo de exibição razor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="df9f9-131">Sintaxe:</span><span class="sxs-lookup"><span data-stu-id="df9f9-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="df9f9-132">O auxiliar de marca de entrada:</span><span class="sxs-lookup"><span data-stu-id="df9f9-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="df9f9-133">Gera o `id` e `name` atributos HTML para o nome da expressão especificada no `asp-for` atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="df9f9-134">`asp-for="Property1.Property2"` equivale a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="df9f9-135">O nome da expressão é o que é usado para o `asp-for` valor do atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="df9f9-136">Consulte o [nomes de expressão](#expression-names) seção para obter informações adicionais.</span><span class="sxs-lookup"><span data-stu-id="df9f9-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="df9f9-137">Define o HTML `type` com base no tipo de modelo de valor de atributo e [anotação de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados para a propriedade de modelo</span><span class="sxs-lookup"><span data-stu-id="df9f9-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="df9f9-138">Não substitua o HTML `type` valor de atributo quando um for especificado</span><span class="sxs-lookup"><span data-stu-id="df9f9-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="df9f9-139">Gera [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributos de validação de [anotação de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados às propriedades de modelo</span><span class="sxs-lookup"><span data-stu-id="df9f9-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="df9f9-140">Tem um recurso auxiliar HTML sobrepõe `Html.TextBoxFor` e `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="df9f9-141">Consulte o **alternativas de auxiliar HTML para auxiliar de marca de entrada** seção para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="df9f9-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="df9f9-142">Fornece alta segurança de tipos.</span><span class="sxs-lookup"><span data-stu-id="df9f9-142">Provides strong typing.</span></span> <span data-ttu-id="df9f9-143">Se o nome das alterações de propriedade e você não atualizar o auxiliar de marca você obterá um erro semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="df9f9-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="df9f9-144">O `Input` auxiliar de marca define o HTML `type` atributo com base no tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="df9f9-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="df9f9-145">A tabela a seguir lista alguns tipos comuns de .NET e o tipo HTML gerado (não todos os tipos de .NET é listado).</span><span class="sxs-lookup"><span data-stu-id="df9f9-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="df9f9-146">Tipo .NET</span><span class="sxs-lookup"><span data-stu-id="df9f9-146">.NET type</span></span>|<span data-ttu-id="df9f9-147">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="df9f9-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="df9f9-148">bool</span><span class="sxs-lookup"><span data-stu-id="df9f9-148">Bool</span></span>|<span data-ttu-id="df9f9-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="df9f9-149">type=”checkbox”</span></span>|
|<span data-ttu-id="df9f9-150">Cadeia de Caracteres</span><span class="sxs-lookup"><span data-stu-id="df9f9-150">String</span></span>|<span data-ttu-id="df9f9-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="df9f9-151">type=”text”</span></span>|
|<span data-ttu-id="df9f9-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="df9f9-152">DateTime</span></span>|<span data-ttu-id="df9f9-153">tipo = "datetime"</span><span class="sxs-lookup"><span data-stu-id="df9f9-153">type=”datetime”</span></span>|
|<span data-ttu-id="df9f9-154">Byte</span><span class="sxs-lookup"><span data-stu-id="df9f9-154">Byte</span></span>|<span data-ttu-id="df9f9-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="df9f9-155">type=”number”</span></span>|
|<span data-ttu-id="df9f9-156">int</span><span class="sxs-lookup"><span data-stu-id="df9f9-156">Int</span></span>|<span data-ttu-id="df9f9-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="df9f9-157">type=”number”</span></span>|
|<span data-ttu-id="df9f9-158">Single e Double</span><span class="sxs-lookup"><span data-stu-id="df9f9-158">Single, Double</span></span>|<span data-ttu-id="df9f9-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="df9f9-159">type=”number”</span></span>|


<span data-ttu-id="df9f9-160">A tabela a seguir mostra algumas [as anotações de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos que o auxiliar de marca de entrada serão mapeados para tipos específicos de entrada (não todos os atributos de validação é listado):</span><span class="sxs-lookup"><span data-stu-id="df9f9-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="df9f9-161">Atributo</span><span class="sxs-lookup"><span data-stu-id="df9f9-161">Attribute</span></span>|<span data-ttu-id="df9f9-162">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="df9f9-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="df9f9-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="df9f9-163">[EmailAddress]</span></span>|<span data-ttu-id="df9f9-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="df9f9-164">type=”email”</span></span>|
|<span data-ttu-id="df9f9-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="df9f9-165">[Url]</span></span>|<span data-ttu-id="df9f9-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="df9f9-166">type=”url”</span></span>|
|<span data-ttu-id="df9f9-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="df9f9-167">[HiddenInput]</span></span>|<span data-ttu-id="df9f9-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="df9f9-168">type=”hidden”</span></span>|
|<span data-ttu-id="df9f9-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="df9f9-169">[Phone]</span></span>|<span data-ttu-id="df9f9-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="df9f9-170">type=”tel”</span></span>|
|<span data-ttu-id="df9f9-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="df9f9-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="df9f9-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="df9f9-172">type=”password”</span></span>|
|<span data-ttu-id="df9f9-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="df9f9-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="df9f9-174">tipo = "Data"</span><span class="sxs-lookup"><span data-stu-id="df9f9-174">type=”date”</span></span>|
|<span data-ttu-id="df9f9-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="df9f9-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="df9f9-176">tipo = "Hora"</span><span class="sxs-lookup"><span data-stu-id="df9f9-176">type=”time”</span></span>|


<span data-ttu-id="df9f9-177">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="df9f9-178">O código anterior gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="df9f9-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="df9f9-179">As anotações de dados aplicadas para o `Email` e `Password` propriedades geram metadados no modelo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="df9f9-180">O auxiliar de marca de entrada consome os metadados do modelo e produz [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributos (consulte [validação de modelo](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="df9f9-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="df9f9-181">Esses atributos descrevem os validadores para anexar a campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="df9f9-182">Isso fornece discreto HTML5 e [jQuery](https://jquery.com/) validação.</span><span class="sxs-lookup"><span data-stu-id="df9f9-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="df9f9-183">Os atributos discretas tem o formato `data-val-rule="Error Message"`, em que a regra é o nome da regra de validação (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) Se uma mensagem de erro é fornecida no atributo, ele será exibido como o valor para o `data-val-rule` atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="df9f9-184">Também há atributos do formulário `data-val-ruleName-argumentName="argumentValue"` que fornecem detalhes adicionais sobre a regra, por exemplo, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="df9f9-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="df9f9-185">Alternativas de auxiliar HTML para auxiliar de marca de entrada</span><span class="sxs-lookup"><span data-stu-id="df9f9-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="df9f9-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` ter sobreposição de recursos com o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="df9f9-187">O auxiliar de marca de entrada definirá automaticamente a `type` atributo; `Html.TextBox` e `Html.TextBoxFor` não.</span><span class="sxs-lookup"><span data-stu-id="df9f9-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="df9f9-188">`Html.Editor`e `Html.EditorFor` manipular coleções, objetos complexos e modelos; não o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="df9f9-189">O auxiliar de marca de entrada, `Html.EditorFor` e `Html.TextBoxFor` são fortemente tipadas (eles usar expressões lambda); `Html.TextBox` e `Html.Editor` não são (usarem nomes de expressão).</span><span class="sxs-lookup"><span data-stu-id="df9f9-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="df9f9-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="df9f9-190">HtmlAttributes</span></span>

<span data-ttu-id="df9f9-191">`@Html.Editor()`e `@Html.EditorFor()` usar especial `ViewDataDictionary` entrada denominada `htmlAttributes` durante a execução de seus modelos padrão.</span><span class="sxs-lookup"><span data-stu-id="df9f9-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="df9f9-192">Esse comportamento é aumentado opcionalmente usando `additionalViewData` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="df9f9-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="df9f9-193">A chave "htmlAttributes" diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="df9f9-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="df9f9-194">A chave "htmlAttributes" é tratada da mesma forma que o `htmlAttributes` objeto passado para entrada auxiliares como `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="df9f9-195">Nomes de expressão</span><span class="sxs-lookup"><span data-stu-id="df9f9-195">Expression names</span></span>

<span data-ttu-id="df9f9-196">O `asp-for` valor de atributo é um `ModelExpression` e o lado direito de uma expressão lambda.</span><span class="sxs-lookup"><span data-stu-id="df9f9-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="df9f9-197">Portanto, `asp-for="Property1"` se torna `m => m.Property1` no código gerado que é por isso você não precisa com prefixo `Model`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="df9f9-198">Você pode usar o "@" caractere para iniciar uma expressão embutido e mover antes do `m.`:</span><span class="sxs-lookup"><span data-stu-id="df9f9-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="df9f9-199">Gera o seguinte:</span><span class="sxs-lookup"><span data-stu-id="df9f9-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="df9f9-200">Com as propriedades de coleção, `asp-for="CollectionProperty[23].Member"` gera o mesmo nome como `asp-for="CollectionProperty[i].Member"` quando `i` tem o valor `23`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="df9f9-201">Navegando propriedades filho</span><span class="sxs-lookup"><span data-stu-id="df9f9-201">Navigating child properties</span></span>

<span data-ttu-id="df9f9-202">Você também pode navegar para propriedades filho usando o caminho da propriedade do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="df9f9-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="df9f9-203">Considere a possibilidade de uma classe de modelo mais complexa que contém um filho `Address` propriedade.</span><span class="sxs-lookup"><span data-stu-id="df9f9-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="df9f9-204">No modo de exibição, podemos associar a `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="df9f9-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="df9f9-205">O HTML a seguir é gerado para `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="df9f9-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="df9f9-206">Nomes de expressão e coleções</span><span class="sxs-lookup"><span data-stu-id="df9f9-206">Expression names and Collections</span></span>

<span data-ttu-id="df9f9-207">Exemplo de um modelo que contém uma matriz de `Colors`:</span><span class="sxs-lookup"><span data-stu-id="df9f9-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="df9f9-208">O método de ação:</span><span class="sxs-lookup"><span data-stu-id="df9f9-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="df9f9-209">O Razor a seguir mostra como você acessa um determinado `Color` elemento:</span><span class="sxs-lookup"><span data-stu-id="df9f9-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="df9f9-210">O *Views/Shared/EditorTemplates/String.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="df9f9-211">Exemplo usando `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="df9f9-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="df9f9-212">O Razor a seguir mostra como iterar em uma coleção:</span><span class="sxs-lookup"><span data-stu-id="df9f9-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="df9f9-213">O *Views/Shared/EditorTemplates/ToDoItem.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="df9f9-214">Sempre use `for` (e *não* `foreach`) para iterar sobre uma lista.</span><span class="sxs-lookup"><span data-stu-id="df9f9-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="df9f9-215">Expressão avaliar um indexador em um LINQ pode ser caro e deve ser minimizada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="df9f9-216">O código de exemplo comentado acima mostra como você poderia substituir a expressão lambda com o `@` operador para acessar cada `ToDoItem` na lista.</span><span class="sxs-lookup"><span data-stu-id="df9f9-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="df9f9-217">O auxiliar de marca de área de texto</span><span class="sxs-lookup"><span data-stu-id="df9f9-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="df9f9-218">O `Textarea Tag Helper` auxiliar de marca é semelhante para o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="df9f9-219">Gera o `id` e `name` atributos e os atributos de validação de dados do modelo para um [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="df9f9-220">Fornece alta segurança de tipos.</span><span class="sxs-lookup"><span data-stu-id="df9f9-220">Provides strong typing.</span></span>

* <span data-ttu-id="df9f9-221">Alternativa de auxiliar HTML:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="df9f9-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="df9f9-222">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="df9f9-223">O HTML a seguir é gerado:</span><span class="sxs-lookup"><span data-stu-id="df9f9-223">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="df9f9-224">O auxiliar de marca de rótulo</span><span class="sxs-lookup"><span data-stu-id="df9f9-224">The Label Tag Helper</span></span>

* <span data-ttu-id="df9f9-225">Gera a legenda do rótulo e `for` de atributo em uma [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elemento para um nome de expressão</span><span class="sxs-lookup"><span data-stu-id="df9f9-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="df9f9-226">Alternativa de auxiliar HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="df9f9-227">O `Label Tag Helper` fornece os seguintes benefícios sobre um elemento de rótulo HTML puro:</span><span class="sxs-lookup"><span data-stu-id="df9f9-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="df9f9-228">Você obtém automaticamente o valor de rótulo descritivo do `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="df9f9-229">O nome de exibição desejado pode mudar com o tempo e a combinação de `Display` auxiliar de marca de rótulo e de atributo se aplicará a `Display` em qualquer lugar, ele é usado.</span><span class="sxs-lookup"><span data-stu-id="df9f9-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="df9f9-230">Menos marcação no código-fonte</span><span class="sxs-lookup"><span data-stu-id="df9f9-230">Less markup in source code</span></span>

* <span data-ttu-id="df9f9-231">Forte digitando com a propriedade de modelo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-231">Strong typing with the model property.</span></span>

<span data-ttu-id="df9f9-232">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="df9f9-233">O HTML a seguir é gerado para o `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="df9f9-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="df9f9-234">O auxiliar de marca de rótulo gerado o `for` valor do atributo de "Email", que é a ID associada a `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="df9f9-235">Os auxiliares de marca gerar consistente `id` e `for` elementos para que eles possam ser corretamente associados.</span><span class="sxs-lookup"><span data-stu-id="df9f9-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="df9f9-236">A legenda neste exemplo é proveniente do `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="df9f9-237">Se o modelo não contém um `Display` atributo, a legenda deve ser o nome da propriedade da expressão.</span><span class="sxs-lookup"><span data-stu-id="df9f9-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="df9f9-238">Os auxiliares de marca de validação</span><span class="sxs-lookup"><span data-stu-id="df9f9-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="df9f9-239">Há dois auxiliares de marcação de validação.</span><span class="sxs-lookup"><span data-stu-id="df9f9-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="df9f9-240">O `Validation Message Tag Helper` (que exibe uma mensagem de validação para uma única propriedade em seu modelo) e o `Validation Summary Tag Helper` (que exibe um resumo de erros de validação).</span><span class="sxs-lookup"><span data-stu-id="df9f9-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="df9f9-241">O `Input Tag Helper` adiciona atributos de validação do HTML5 cliente lado para elementos com base em dados de atributos de anotação em suas classes de modelo de entrada.</span><span class="sxs-lookup"><span data-stu-id="df9f9-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="df9f9-242">Além disso, a validação é executada no servidor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-242">Validation is also performed on the server.</span></span> <span data-ttu-id="df9f9-243">O auxiliar de marca de validação exibe essas mensagens de erro quando ocorre um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="df9f9-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="df9f9-244">O auxiliar de marca de mensagem de validação</span><span class="sxs-lookup"><span data-stu-id="df9f9-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="df9f9-245">Adiciona o [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` de atributo para o [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, que anexa as mensagens de erro de validação no campo de entrada da propriedade do modelo especificado.</span><span class="sxs-lookup"><span data-stu-id="df9f9-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="df9f9-246">Quando ocorre um erro de validação do lado cliente, [jQuery](https://jquery.com/) exibe a mensagem de erro no `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="df9f9-247">Também, a validação é feita no servidor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-247">Validation also takes place on the server.</span></span> <span data-ttu-id="df9f9-248">Os clientes poderão ter JavaScript desabilitado e só pode ser feita alguma validação no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="df9f9-249">Alternativa de auxiliar HTML:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="df9f9-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="df9f9-250">O `Validation Message Tag Helper` é usado com o `asp-validation-for` atributo em um HTML [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="df9f9-251">O auxiliar de marca de mensagem de validação irá gerar o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="df9f9-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="df9f9-252">Você geralmente usa o `Validation Message Tag Helper` após um `Input` auxiliar de marca para a mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="df9f9-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="df9f9-253">Isso exibe as mensagens de erro de validação próximo a entrada que causou o erro.</span><span class="sxs-lookup"><span data-stu-id="df9f9-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="df9f9-254">Você deve ter uma exibição com o JavaScript correto e [jQuery](https://jquery.com/) script referências em vigor para a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="df9f9-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="df9f9-255">Consulte [validação de modelo](../models/validation.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="df9f9-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="df9f9-256">Quando ocorre um erro de validação de lado de servidor (por exemplo quando você tem a validação do lado do servidor personalizado ou validação do lado do cliente é desabilitada), o MVC coloca essa mensagem de erro como o corpo do `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="df9f9-257">O auxiliar de marca de resumo de validação</span><span class="sxs-lookup"><span data-stu-id="df9f9-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="df9f9-258">Destinos `<div>` elementos com o `asp-validation-summary` atributo</span><span class="sxs-lookup"><span data-stu-id="df9f9-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="df9f9-259">Alternativa de auxiliar HTML:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="df9f9-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="df9f9-260">O `Validation Summary Tag Helper` é usado para exibir um resumo das mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="df9f9-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="df9f9-261">O `asp-validation-summary` valor de atributo pode ser qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="df9f9-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="df9f9-262">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="df9f9-262">asp-validation-summary</span></span>|<span data-ttu-id="df9f9-263">Mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="df9f9-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="df9f9-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="df9f9-264">ValidationSummary.All</span></span>|<span data-ttu-id="df9f9-265">Nível de propriedade e o modelo</span><span class="sxs-lookup"><span data-stu-id="df9f9-265">Property and model level</span></span>|
|<span data-ttu-id="df9f9-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="df9f9-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="df9f9-267">Modelo</span><span class="sxs-lookup"><span data-stu-id="df9f9-267">Model</span></span>|
|<span data-ttu-id="df9f9-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="df9f9-268">ValidationSummary.None</span></span>|<span data-ttu-id="df9f9-269">Nenhum</span><span class="sxs-lookup"><span data-stu-id="df9f9-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="df9f9-270">Amostra</span><span class="sxs-lookup"><span data-stu-id="df9f9-270">Sample</span></span>

<span data-ttu-id="df9f9-271">No exemplo a seguir, o modelo de dados é decorado com `DataAnnotation` atributos, que gera mensagens de erro de validação no `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="df9f9-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="df9f9-272">Quando ocorre um erro de validação, o auxiliar de marca de validação exibe a mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="df9f9-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="df9f9-273">O código HTML gerado (quando o modelo é válido):</span><span class="sxs-lookup"><span data-stu-id="df9f9-273">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="df9f9-274">O auxiliar de marca de seleção</span><span class="sxs-lookup"><span data-stu-id="df9f9-274">The Select Tag Helper</span></span>

* <span data-ttu-id="df9f9-275">Gera [selecione](https://www.w3.org/wiki/HTML/Elements/select) e respectivos [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos para as propriedades do modelo.</span><span class="sxs-lookup"><span data-stu-id="df9f9-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="df9f9-276">Tem uma alternativa de auxiliar HTML `Html.DropDownListFor` e`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="df9f9-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="df9f9-277">O `Select Tag Helper` `asp-for` Especifica o nome da propriedade de modelo para o [selecione](https://www.w3.org/wiki/HTML/Elements/select) elemento e `asp-items` Especifica o [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos.</span><span class="sxs-lookup"><span data-stu-id="df9f9-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="df9f9-278">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="df9f9-279">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="df9f9-280">O `Index` método inicializa o `CountryViewModel`, define o país/região selecionado e o transmite para o `Index` exibição.</span><span class="sxs-lookup"><span data-stu-id="df9f9-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="df9f9-281">O HTTP POST `Index` método exibe a seleção:</span><span class="sxs-lookup"><span data-stu-id="df9f9-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="df9f9-282">O `Index` exibição:</span><span class="sxs-lookup"><span data-stu-id="df9f9-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="df9f9-283">Que gera o seguinte HTML (com "CA" selecionada):</span><span class="sxs-lookup"><span data-stu-id="df9f9-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="df9f9-284">Não é recomendável usar `ViewBag` ou `ViewData` com o auxiliar de marca selecionar.</span><span class="sxs-lookup"><span data-stu-id="df9f9-284">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="df9f9-285">Um modelo de exibição é mais robusto fornecer metadados MVC e geralmente menos problemáticos.</span><span class="sxs-lookup"><span data-stu-id="df9f9-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="df9f9-286">O `asp-for` valor de atributo é um caso especial e não requer um `Model` prefixo, os outros não de atributos do auxiliar de marcação (como `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="df9f9-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="df9f9-287">Associação de enum</span><span class="sxs-lookup"><span data-stu-id="df9f9-287">Enum binding</span></span>

<span data-ttu-id="df9f9-288">Geralmente é conveniente usar `<select>` com um `enum` propriedade e gerar o `SelectListItem` elementos do `enum` valores.</span><span class="sxs-lookup"><span data-stu-id="df9f9-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="df9f9-289">Amostra:</span><span class="sxs-lookup"><span data-stu-id="df9f9-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="df9f9-290">O `GetEnumSelectList` método gera uma `SelectList` objeto para um enum.</span><span class="sxs-lookup"><span data-stu-id="df9f9-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="df9f9-291">Possível decorar o enumerador de lista com o `Display` atributo para obter uma interface de usuário mais rica:</span><span class="sxs-lookup"><span data-stu-id="df9f9-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="df9f9-292">O HTML a seguir é gerado:</span><span class="sxs-lookup"><span data-stu-id="df9f9-292">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="df9f9-293">Opção de grupo</span><span class="sxs-lookup"><span data-stu-id="df9f9-293">Option Group</span></span>

<span data-ttu-id="df9f9-294">O HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento é gerado quando o modelo de exibição contém um ou mais `SelectListGroup` objetos.</span><span class="sxs-lookup"><span data-stu-id="df9f9-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="df9f9-295">O `CountryViewModelGroup` grupos a `SelectListItem` elementos nos grupos "América do Norte" e "Europa":</span><span class="sxs-lookup"><span data-stu-id="df9f9-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="df9f9-296">Os dois grupos são mostrados abaixo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-296">The two groups are shown below:</span></span>

![exemplo de grupo de opção](working-with-forms/_static/grp.png)

<span data-ttu-id="df9f9-298">O código HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="df9f9-298">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="df9f9-299">Seleção múltipla</span><span class="sxs-lookup"><span data-stu-id="df9f9-299">Multiple select</span></span>

<span data-ttu-id="df9f9-300">O auxiliar de marca selecione gerará automaticamente o [vários = "várias"](http://w3c.github.io/html-reference/select.html) atributo se a propriedade especificada a `asp-for` atributo é um `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="df9f9-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="df9f9-301">Por exemplo, considerando o seguinte modelo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="df9f9-302">Com a seguinte exibição:</span><span class="sxs-lookup"><span data-stu-id="df9f9-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="df9f9-303">Gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="df9f9-303">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="df9f9-304">Nenhuma seleção</span><span class="sxs-lookup"><span data-stu-id="df9f9-304">No selection</span></span>

<span data-ttu-id="df9f9-305">Se você estiver usando a opção "não especificada" em várias páginas, você pode criar um modelo para eliminar o HTML de repetição:</span><span class="sxs-lookup"><span data-stu-id="df9f9-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="df9f9-306">O *Views/Shared/EditorTemplates/CountryViewModel.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="df9f9-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="df9f9-307">Adicionando HTML [ \<opção >](https://www.w3.org/wiki/HTML/Elements/option) elementos não está limitado ao *nenhuma seleção* caso.</span><span class="sxs-lookup"><span data-stu-id="df9f9-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="df9f9-308">Por exemplo, o seguinte método de exibição e a ação irá gerar HTML com o código acima:</span><span class="sxs-lookup"><span data-stu-id="df9f9-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="df9f9-309">Corretas `<option>` elemento será selecionado (contêm o `selected="selected"` atributo) dependendo atual `Country` valor.</span><span class="sxs-lookup"><span data-stu-id="df9f9-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="df9f9-310">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="df9f9-310">Additional Resources</span></span>

* [<span data-ttu-id="df9f9-311">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="df9f9-311">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="df9f9-312">Elemento de formulário HTML</span><span class="sxs-lookup"><span data-stu-id="df9f9-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="df9f9-313">Solicitação de Token de verificação</span><span class="sxs-lookup"><span data-stu-id="df9f9-313">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="df9f9-314">Associação de modelos</span><span class="sxs-lookup"><span data-stu-id="df9f9-314">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="df9f9-315">Validação de modelo</span><span class="sxs-lookup"><span data-stu-id="df9f9-315">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="df9f9-316">anotações de dados</span><span class="sxs-lookup"><span data-stu-id="df9f9-316">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="df9f9-317">[Trechos de código para este documento de código](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="df9f9-317">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
