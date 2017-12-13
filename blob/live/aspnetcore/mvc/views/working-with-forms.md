---
title: "Auxiliares de marcação em formulários do ASP.NET Core"
author: rick-anderson
description: "Descreve interno de auxiliares de marcação usados com formulários."
keywords: "Formulários do ASP.NET Core, o auxiliar de marca, TagHelper, do formulário HTML,"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: da36985206521798d3bfe71f6372dc5cc4fca09a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="6a436-104">Introdução ao uso de auxiliares de marcação em formulários do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a436-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="6a436-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), e [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="6a436-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="6a436-106">Este documento demonstra trabalhar com formulários e os elementos HTML comumente usados em um formulário.</span><span class="sxs-lookup"><span data-stu-id="6a436-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="6a436-107">O HTML [formulário](https://www.w3.org/TR/html401/interact/forms.html) elemento fornece o uso de aplicativos web mecanismo principal para enviar dados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="6a436-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="6a436-108">A maioria deste documento descreve [auxiliares de marcação](tag-helpers/intro.md) e como eles podem ajudar você a criar produtiva robustos formulários HTML.</span><span class="sxs-lookup"><span data-stu-id="6a436-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="6a436-109">É recomendável que você leia [Introdução ao auxiliares de marcação](tag-helpers/intro.md) antes de ler este documento.</span><span class="sxs-lookup"><span data-stu-id="6a436-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="6a436-110">Em muitos casos, auxiliares HTML fornecem uma abordagem alternativa para um auxiliar de marca específica, mas é importante reconhecer que auxiliares de marcação não substituem auxiliares HTML e não é um auxiliar de marca para cada auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="6a436-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="6a436-111">Quando existe uma alternativa de auxiliar HTML, ele é mencionado.</span><span class="sxs-lookup"><span data-stu-id="6a436-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="6a436-112">O auxiliar de marca de formulário</span><span class="sxs-lookup"><span data-stu-id="6a436-112">The Form Tag Helper</span></span>

<span data-ttu-id="6a436-113">O [formulário](https://www.w3.org/TR/html401/interact/forms.html) auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="6a436-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="6a436-114">Gera o HTML [ \<formulário >](https://www.w3.org/TR/html401/interact/forms.html) `action` valor de atributo para uma ação do controlador MVC ou uma rota nomeada</span><span class="sxs-lookup"><span data-stu-id="6a436-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="6a436-115">Gera um oculta [solicitação de Token de verificação](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="6a436-115">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="6a436-116">Fornece o `asp-route-<Parameter Name>` atributo, onde `<Parameter Name>` é adicionado aos valores de rota.</span><span class="sxs-lookup"><span data-stu-id="6a436-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="6a436-117">O `routeValues` parâmetros para `Html.BeginForm` e `Html.BeginRouteForm` fornecem funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="6a436-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="6a436-118">Tem uma alternativa de auxiliar HTML `Html.BeginForm` e`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="6a436-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="6a436-119">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-119">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="6a436-120">O auxiliar de marca de formulário acima gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6a436-120">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="6a436-121">O tempo de execução do MVC gera o `action` valor do atributo dos atributos do auxiliar de marca de formulário `asp-controller` e `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="6a436-121">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="6a436-122">O auxiliar de marca de formulário também gera oculto [solicitação de Token de verificação](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) para evitar a falsificação de solicitação entre sites (quando usado com o `[ValidateAntiForgeryToken]` atributo no método de ação HTTP Post).</span><span class="sxs-lookup"><span data-stu-id="6a436-122">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="6a436-123">Proteger um formulário HTML puro contra falsificação de solicitação entre sites é difícil, o auxiliar de marca de formulário fornece este serviço para você.</span><span class="sxs-lookup"><span data-stu-id="6a436-123">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="6a436-124">Usando uma rota nomeada</span><span class="sxs-lookup"><span data-stu-id="6a436-124">Using a named route</span></span>

<span data-ttu-id="6a436-125">O `asp-route` atributo do auxiliar de marca também pode gerar a marcação para o HTML `action` atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-125">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="6a436-126">Um aplicativo com um [rota](../../fundamentals/routing.md) chamado `register` poderia usar a seguinte marcação para a página de registro:</span><span class="sxs-lookup"><span data-stu-id="6a436-126">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="6a436-127">Muitas das exibições no *modos de exibição/conta* pasta (gerado quando você cria um novo aplicativo web com *contas de usuário individuais*) contêm o [asp de rota de returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) atributo:</span><span class="sxs-lookup"><span data-stu-id="6a436-127">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="6a436-128">Com os modelos internos, `returnUrl` só é preenchida automaticamente quando você tentar acessar um recurso autorizado, mas não é autenticado ou autorizado.</span><span class="sxs-lookup"><span data-stu-id="6a436-128">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="6a436-129">Quando você tenta um acesso não autorizado, o middleware de segurança redireciona para a página de logon com o `returnUrl` definido.</span><span class="sxs-lookup"><span data-stu-id="6a436-129">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="6a436-130">O auxiliar de marca de entrada</span><span class="sxs-lookup"><span data-stu-id="6a436-130">The Input Tag Helper</span></span>

<span data-ttu-id="6a436-131">O auxiliar de marca de entrada associa um HTML [ \<entrada >](https://www.w3.org/wiki/HTML/Elements/input) elemento para uma expressão de modelo no modo de exibição razor.</span><span class="sxs-lookup"><span data-stu-id="6a436-131">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="6a436-132">Sintaxe:</span><span class="sxs-lookup"><span data-stu-id="6a436-132">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="6a436-133">O auxiliar de marca de entrada:</span><span class="sxs-lookup"><span data-stu-id="6a436-133">The Input Tag Helper:</span></span>

* <span data-ttu-id="6a436-134">Gera o `id` e `name` atributos HTML para o nome da expressão especificada no `asp-for` atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-134">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="6a436-135">`asp-for="Property1.Property2"` equivale a `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="6a436-135">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="6a436-136">O nome da expressão é o que é usado para o `asp-for` valor do atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-136">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="6a436-137">Consulte o [nomes de expressão](#expression-names) seção para obter informações adicionais.</span><span class="sxs-lookup"><span data-stu-id="6a436-137">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="6a436-138">Define o HTML `type` com base no tipo de modelo de valor de atributo e [anotação de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados para a propriedade de modelo</span><span class="sxs-lookup"><span data-stu-id="6a436-138">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="6a436-139">Não substituirá o HTML `type` valor de atributo quando um for especificado</span><span class="sxs-lookup"><span data-stu-id="6a436-139">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="6a436-140">Gera [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) atributos de validação de [anotação de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos aplicados às propriedades de modelo</span><span class="sxs-lookup"><span data-stu-id="6a436-140">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="6a436-141">Tem um recurso auxiliar HTML sobrepõe `Html.TextBoxFor` e `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="6a436-141">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="6a436-142">Consulte o **alternativas de auxiliar HTML para auxiliar de marca de entrada** seção para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="6a436-142">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="6a436-143">Fornece alta segurança de tipos.</span><span class="sxs-lookup"><span data-stu-id="6a436-143">Provides strong typing.</span></span> <span data-ttu-id="6a436-144">Se o nome das alterações de propriedade e você não atualizar o auxiliar de marca você obterá um erro semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="6a436-144">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="6a436-145">O `Input` auxiliar de marca define o HTML `type` atributo com base no tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="6a436-145">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="6a436-146">A tabela a seguir lista alguns tipos comuns de .NET e o tipo HTML gerado (não todos os tipos de .NET é listado).</span><span class="sxs-lookup"><span data-stu-id="6a436-146">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="6a436-147">Tipo .NET</span><span class="sxs-lookup"><span data-stu-id="6a436-147">.NET type</span></span>|<span data-ttu-id="6a436-148">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="6a436-148">Input Type</span></span>|
|---|---|
|<span data-ttu-id="6a436-149">bool</span><span class="sxs-lookup"><span data-stu-id="6a436-149">Bool</span></span>|<span data-ttu-id="6a436-150">tipo = "caixa de seleção"</span><span class="sxs-lookup"><span data-stu-id="6a436-150">type=”checkbox”</span></span>|
|<span data-ttu-id="6a436-151">Cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="6a436-151">String</span></span>|<span data-ttu-id="6a436-152">tipo = "text"</span><span class="sxs-lookup"><span data-stu-id="6a436-152">type=”text”</span></span>|
|<span data-ttu-id="6a436-153">DateTime</span><span class="sxs-lookup"><span data-stu-id="6a436-153">DateTime</span></span>|<span data-ttu-id="6a436-154">tipo = "datetime"</span><span class="sxs-lookup"><span data-stu-id="6a436-154">type=”datetime”</span></span>|
|<span data-ttu-id="6a436-155">Byte</span><span class="sxs-lookup"><span data-stu-id="6a436-155">Byte</span></span>|<span data-ttu-id="6a436-156">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="6a436-156">type=”number”</span></span>|
|<span data-ttu-id="6a436-157">int</span><span class="sxs-lookup"><span data-stu-id="6a436-157">Int</span></span>|<span data-ttu-id="6a436-158">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="6a436-158">type=”number”</span></span>|
|<span data-ttu-id="6a436-159">Single e Double</span><span class="sxs-lookup"><span data-stu-id="6a436-159">Single, Double</span></span>|<span data-ttu-id="6a436-160">tipo = "number"</span><span class="sxs-lookup"><span data-stu-id="6a436-160">type=”number”</span></span>|


<span data-ttu-id="6a436-161">A tabela a seguir mostra algumas [as anotações de dados](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atributos que o auxiliar de marca de entrada serão mapeados para tipos específicos de entrada (não todos os atributos de validação é listado):</span><span class="sxs-lookup"><span data-stu-id="6a436-161">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="6a436-162">Atributo</span><span class="sxs-lookup"><span data-stu-id="6a436-162">Attribute</span></span>|<span data-ttu-id="6a436-163">Tipo de entrada</span><span class="sxs-lookup"><span data-stu-id="6a436-163">Input Type</span></span>|
|---|---|
|<span data-ttu-id="6a436-164">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="6a436-164">[EmailAddress]</span></span>|<span data-ttu-id="6a436-165">tipo = "email"</span><span class="sxs-lookup"><span data-stu-id="6a436-165">type=”email”</span></span>|
|<span data-ttu-id="6a436-166">[Url]</span><span class="sxs-lookup"><span data-stu-id="6a436-166">[Url]</span></span>|<span data-ttu-id="6a436-167">tipo = "url"</span><span class="sxs-lookup"><span data-stu-id="6a436-167">type=”url”</span></span>|
|<span data-ttu-id="6a436-168">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="6a436-168">[HiddenInput]</span></span>|<span data-ttu-id="6a436-169">tipo = "oculto"</span><span class="sxs-lookup"><span data-stu-id="6a436-169">type=”hidden”</span></span>|
|<span data-ttu-id="6a436-170">[Phone]</span><span class="sxs-lookup"><span data-stu-id="6a436-170">[Phone]</span></span>|<span data-ttu-id="6a436-171">tipo = "telefone"</span><span class="sxs-lookup"><span data-stu-id="6a436-171">type=”tel”</span></span>|
|<span data-ttu-id="6a436-172">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="6a436-172">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="6a436-173">tipo = "senha"</span><span class="sxs-lookup"><span data-stu-id="6a436-173">type=”password”</span></span>|
|<span data-ttu-id="6a436-174">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="6a436-174">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="6a436-175">tipo = "Data"</span><span class="sxs-lookup"><span data-stu-id="6a436-175">type=”date”</span></span>|
|<span data-ttu-id="6a436-176">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="6a436-176">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="6a436-177">tipo = "Hora"</span><span class="sxs-lookup"><span data-stu-id="6a436-177">type=”time”</span></span>|


<span data-ttu-id="6a436-178">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-178">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="6a436-179">O código anterior gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6a436-179">The code above generates the following HTML:</span></span>

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

<span data-ttu-id="6a436-180">As anotações de dados aplicadas para o `Email` e `Password` propriedades geram metadados no modelo.</span><span class="sxs-lookup"><span data-stu-id="6a436-180">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="6a436-181">O auxiliar de marca de entrada consome os metadados do modelo e produz [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atributos (consulte [validação de modelo](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="6a436-181">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="6a436-182">Esses atributos descrevem os validadores para anexar a campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a436-182">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="6a436-183">Isso fornece discreto HTML5 e [jQuery](https://jquery.com/) validação.</span><span class="sxs-lookup"><span data-stu-id="6a436-183">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="6a436-184">Os atributos discretas tem o formato `data-val-rule="Error Message"`, em que a regra é o nome da regra de validação (como `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) Se uma mensagem de erro é fornecida no atributo, ele será exibido como o valor para o `data-val-rule` atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-184">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="6a436-185">Também há atributos do formulário `data-val-ruleName-argumentName="argumentValue"` que fornecem detalhes adicionais sobre a regra, por exemplo, `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="6a436-185">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="6a436-186">Alternativas de auxiliar HTML para auxiliar de marca de entrada</span><span class="sxs-lookup"><span data-stu-id="6a436-186">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="6a436-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` e `Html.EditorFor` ter sobreposição de recursos com o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a436-187">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="6a436-188">O auxiliar de marca de entrada definirá automaticamente a `type` atributo; `Html.TextBox` e `Html.TextBoxFor` não.</span><span class="sxs-lookup"><span data-stu-id="6a436-188">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="6a436-189">`Html.Editor`e `Html.EditorFor` manipular coleções, objetos complexos e modelos; não o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a436-189">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="6a436-190">O auxiliar de marca de entrada, `Html.EditorFor` e `Html.TextBoxFor` são fortemente tipadas (eles usar expressões lambda); `Html.TextBox` e `Html.Editor` não são (usarem nomes de expressão).</span><span class="sxs-lookup"><span data-stu-id="6a436-190">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="6a436-191">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="6a436-191">HtmlAttributes</span></span>

<span data-ttu-id="6a436-192">`@Html.Editor()`e `@Html.EditorFor()` usar especial `ViewDataDictionary` entrada denominada `htmlAttributes` durante a execução de seus modelos padrão.</span><span class="sxs-lookup"><span data-stu-id="6a436-192">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="6a436-193">Esse comportamento é aumentado opcionalmente usando `additionalViewData` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="6a436-193">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="6a436-194">A chave "htmlAttributes" diferencia maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6a436-194">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="6a436-195">A chave "htmlAttributes" é tratada da mesma forma que o `htmlAttributes` objeto passado para entrada auxiliares como `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="6a436-195">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="6a436-196">Nomes de expressão</span><span class="sxs-lookup"><span data-stu-id="6a436-196">Expression names</span></span>

<span data-ttu-id="6a436-197">O `asp-for` valor de atributo é um `ModelExpression` e o lado direito de uma expressão lambda.</span><span class="sxs-lookup"><span data-stu-id="6a436-197">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="6a436-198">Portanto, `asp-for="Property1"` se torna `m => m.Property1` no código gerado que é por isso você não precisa com prefixo `Model`.</span><span class="sxs-lookup"><span data-stu-id="6a436-198">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="6a436-199">Você pode usar o "@" caractere para iniciar uma expressão embutido e mover antes do `m.`:</span><span class="sxs-lookup"><span data-stu-id="6a436-199">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="6a436-200">Gera o seguinte:</span><span class="sxs-lookup"><span data-stu-id="6a436-200">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="6a436-201">Com as propriedades de coleção, `asp-for="CollectionProperty[23].Member"` gera o mesmo nome como `asp-for="CollectionProperty[i].Member"` quando `i` tem o valor `23`.</span><span class="sxs-lookup"><span data-stu-id="6a436-201">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="6a436-202">Navegando propriedades filho</span><span class="sxs-lookup"><span data-stu-id="6a436-202">Navigating child properties</span></span>

<span data-ttu-id="6a436-203">Você também pode navegar para propriedades filho usando o caminho da propriedade do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="6a436-203">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="6a436-204">Considere a possibilidade de uma classe de modelo mais complexa que contém um filho `Address` propriedade.</span><span class="sxs-lookup"><span data-stu-id="6a436-204">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="6a436-205">No modo de exibição, podemos associar a `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="6a436-205">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="6a436-206">O HTML a seguir é gerado para `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="6a436-206">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="6a436-207">Nomes de expressão e coleções</span><span class="sxs-lookup"><span data-stu-id="6a436-207">Expression names and Collections</span></span>

<span data-ttu-id="6a436-208">Exemplo de um modelo que contém uma matriz de `Colors`:</span><span class="sxs-lookup"><span data-stu-id="6a436-208">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="6a436-209">O método de ação:</span><span class="sxs-lookup"><span data-stu-id="6a436-209">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="6a436-210">O Razor a seguir mostra como você acessa um determinado `Color` elemento:</span><span class="sxs-lookup"><span data-stu-id="6a436-210">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="6a436-211">O *Views/Shared/EditorTemplates/String.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="6a436-211">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="6a436-212">Exemplo usando `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="6a436-212">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="6a436-213">O Razor a seguir mostra como iterar em uma coleção:</span><span class="sxs-lookup"><span data-stu-id="6a436-213">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="6a436-214">O *Views/Shared/EditorTemplates/ToDoItem.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="6a436-214">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="6a436-215">Sempre use `for` (e *não* `foreach`) para iterar sobre uma lista.</span><span class="sxs-lookup"><span data-stu-id="6a436-215">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="6a436-216">Expressão avaliar um indexador em um LINQ pode ser caro e deve ser minimizada.</span><span class="sxs-lookup"><span data-stu-id="6a436-216">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="6a436-217">O código de exemplo comentado acima mostra como você poderia substituir a expressão lambda com o `@` operador para acessar cada `ToDoItem` na lista.</span><span class="sxs-lookup"><span data-stu-id="6a436-217">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="6a436-218">O auxiliar de marca de área de texto</span><span class="sxs-lookup"><span data-stu-id="6a436-218">The Textarea Tag Helper</span></span>

<span data-ttu-id="6a436-219">O `Textarea Tag Helper` auxiliar de marca é semelhante para o auxiliar de marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a436-219">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="6a436-220">Gera o `id` e `name` atributos e os atributos de validação de dados do modelo para um [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-220">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="6a436-221">Fornece alta segurança de tipos.</span><span class="sxs-lookup"><span data-stu-id="6a436-221">Provides strong typing.</span></span>

* <span data-ttu-id="6a436-222">Alternativa de auxiliar HTML:`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="6a436-222">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="6a436-223">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-223">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="6a436-224">O HTML a seguir é gerado:</span><span class="sxs-lookup"><span data-stu-id="6a436-224">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="6a436-225">O auxiliar de marca de rótulo</span><span class="sxs-lookup"><span data-stu-id="6a436-225">The Label Tag Helper</span></span>

* <span data-ttu-id="6a436-226">Gera a legenda do rótulo e `for` de atributo em uma [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elemento para um nome de expressão</span><span class="sxs-lookup"><span data-stu-id="6a436-226">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="6a436-227">Alternativa de auxiliar HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="6a436-227">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="6a436-228">O `Label Tag Helper` fornece os seguintes benefícios sobre um elemento de rótulo HTML puro:</span><span class="sxs-lookup"><span data-stu-id="6a436-228">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="6a436-229">Você obtém automaticamente o valor de rótulo descritivo do `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-229">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="6a436-230">O nome de exibição desejado pode mudar com o tempo e a combinação de `Display` auxiliar de marca de rótulo e de atributo se aplicará a `Display` em qualquer lugar, ele é usado.</span><span class="sxs-lookup"><span data-stu-id="6a436-230">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="6a436-231">Menos marcação no código-fonte</span><span class="sxs-lookup"><span data-stu-id="6a436-231">Less markup in source code</span></span>

* <span data-ttu-id="6a436-232">Forte digitando com a propriedade de modelo.</span><span class="sxs-lookup"><span data-stu-id="6a436-232">Strong typing with the model property.</span></span>

<span data-ttu-id="6a436-233">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-233">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="6a436-234">O HTML a seguir é gerado para o `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="6a436-234">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="6a436-235">O auxiliar de marca de rótulo gerado o `for` valor do atributo de "Email", que é a ID associada a `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-235">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="6a436-236">Os auxiliares de marca gerar consistente `id` e `for` elementos para que eles possam ser corretamente associados.</span><span class="sxs-lookup"><span data-stu-id="6a436-236">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="6a436-237">A legenda neste exemplo é proveniente do `Display` atributo.</span><span class="sxs-lookup"><span data-stu-id="6a436-237">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="6a436-238">Se o modelo não contém um `Display` atributo, a legenda deve ser o nome da propriedade da expressão.</span><span class="sxs-lookup"><span data-stu-id="6a436-238">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="6a436-239">Os auxiliares de marca de validação</span><span class="sxs-lookup"><span data-stu-id="6a436-239">The Validation Tag Helpers</span></span>

<span data-ttu-id="6a436-240">Há dois auxiliares de marcação de validação.</span><span class="sxs-lookup"><span data-stu-id="6a436-240">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="6a436-241">O `Validation Message Tag Helper` (que exibe uma mensagem de validação para uma única propriedade em seu modelo) e o `Validation Summary Tag Helper` (que exibe um resumo de erros de validação).</span><span class="sxs-lookup"><span data-stu-id="6a436-241">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="6a436-242">O `Input Tag Helper` adiciona atributos de validação do HTML5 cliente lado para elementos com base em dados de atributos de anotação em suas classes de modelo de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a436-242">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="6a436-243">Além disso, a validação é executada no servidor.</span><span class="sxs-lookup"><span data-stu-id="6a436-243">Validation is also performed on the server.</span></span> <span data-ttu-id="6a436-244">O auxiliar de marca de validação exibe essas mensagens de erro quando ocorre um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="6a436-244">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="6a436-245">O auxiliar de marca de mensagem de validação</span><span class="sxs-lookup"><span data-stu-id="6a436-245">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="6a436-246">Adiciona o [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` de atributo para o [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento, que anexa as mensagens de erro de validação no campo de entrada da propriedade do modelo especificado.</span><span class="sxs-lookup"><span data-stu-id="6a436-246">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="6a436-247">Quando ocorre um erro de validação do lado cliente, [jQuery](https://jquery.com/) exibe a mensagem de erro no `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-247">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="6a436-248">Também, a validação é feita no servidor.</span><span class="sxs-lookup"><span data-stu-id="6a436-248">Validation also takes place on the server.</span></span> <span data-ttu-id="6a436-249">Os clientes poderão ter JavaScript desabilitado e só pode ser feita alguma validação no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="6a436-249">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="6a436-250">Alternativa de auxiliar HTML:`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="6a436-250">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="6a436-251">O `Validation Message Tag Helper` é usado com o `asp-validation-for` atributo em um HTML [abrangem](https://developer.mozilla.org/docs/Web/HTML/Element/span) elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-251">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="6a436-252">O auxiliar de marca de mensagem de validação irá gerar o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6a436-252">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="6a436-253">Você geralmente usa o `Validation Message Tag Helper` após um `Input` auxiliar de marca para a mesma propriedade.</span><span class="sxs-lookup"><span data-stu-id="6a436-253">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="6a436-254">Isso exibe as mensagens de erro de validação próximo a entrada que causou o erro.</span><span class="sxs-lookup"><span data-stu-id="6a436-254">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="6a436-255">Você deve ter uma exibição com o JavaScript correto e [jQuery](https://jquery.com/) script referências em vigor para a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="6a436-255">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="6a436-256">Consulte [validação de modelo](../models/validation.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="6a436-256">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="6a436-257">Quando ocorre um erro de validação de lado de servidor (por exemplo quando você tem a validação do lado do servidor personalizado ou validação do lado do cliente é desabilitada), o MVC coloca essa mensagem de erro como o corpo do `<span>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-257">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="6a436-258">O auxiliar de marca de resumo de validação</span><span class="sxs-lookup"><span data-stu-id="6a436-258">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="6a436-259">Destinos `<div>` elementos com o `asp-validation-summary` atributo</span><span class="sxs-lookup"><span data-stu-id="6a436-259">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="6a436-260">Alternativa de auxiliar HTML:`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="6a436-260">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="6a436-261">O `Validation Summary Tag Helper` é usado para exibir um resumo das mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="6a436-261">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="6a436-262">O `asp-validation-summary` valor de atributo pode ser qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="6a436-262">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="6a436-263">Resumo de validação do ASP</span><span class="sxs-lookup"><span data-stu-id="6a436-263">asp-validation-summary</span></span>|<span data-ttu-id="6a436-264">Mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="6a436-264">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="6a436-265">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="6a436-265">ValidationSummary.All</span></span>|<span data-ttu-id="6a436-266">Nível de propriedade e o modelo</span><span class="sxs-lookup"><span data-stu-id="6a436-266">Property and model level</span></span>|
|<span data-ttu-id="6a436-267">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="6a436-267">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="6a436-268">Modelo</span><span class="sxs-lookup"><span data-stu-id="6a436-268">Model</span></span>|
|<span data-ttu-id="6a436-269">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="6a436-269">ValidationSummary.None</span></span>|<span data-ttu-id="6a436-270">Nenhum</span><span class="sxs-lookup"><span data-stu-id="6a436-270">None</span></span>|

### <a name="sample"></a><span data-ttu-id="6a436-271">Amostra</span><span class="sxs-lookup"><span data-stu-id="6a436-271">Sample</span></span>

<span data-ttu-id="6a436-272">No exemplo a seguir, o modelo de dados é decorado com `DataAnnotation` atributos, que gera mensagens de erro de validação no `<input>` elemento.</span><span class="sxs-lookup"><span data-stu-id="6a436-272">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="6a436-273">Quando ocorre um erro de validação, o auxiliar de marca de validação exibe a mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="6a436-273">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="6a436-274">O código HTML gerado (quando o modelo é válido):</span><span class="sxs-lookup"><span data-stu-id="6a436-274">The generated HTML (when the model is valid):</span></span>

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

## <a name="the-select-tag-helper"></a><span data-ttu-id="6a436-275">O auxiliar de marca de seleção</span><span class="sxs-lookup"><span data-stu-id="6a436-275">The Select Tag Helper</span></span>

* <span data-ttu-id="6a436-276">Gera [selecione](https://www.w3.org/wiki/HTML/Elements/select) e respectivos [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos para as propriedades do modelo.</span><span class="sxs-lookup"><span data-stu-id="6a436-276">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="6a436-277">Tem uma alternativa de auxiliar HTML `Html.DropDownListFor` e`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="6a436-277">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="6a436-278">O `Select Tag Helper` `asp-for` Especifica o nome da propriedade de modelo para o [selecione](https://www.w3.org/wiki/HTML/Elements/select) elemento e `asp-items` Especifica o [opção](https://www.w3.org/wiki/HTML/Elements/option) elementos.</span><span class="sxs-lookup"><span data-stu-id="6a436-278">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="6a436-279">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6a436-279">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="6a436-280">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-280">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="6a436-281">O `Index` método inicializa o `CountryViewModel`, define o país/região selecionado e o transmite para o `Index` exibição.</span><span class="sxs-lookup"><span data-stu-id="6a436-281">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="6a436-282">O HTTP POST `Index` método exibe a seleção:</span><span class="sxs-lookup"><span data-stu-id="6a436-282">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="6a436-283">O `Index` exibição:</span><span class="sxs-lookup"><span data-stu-id="6a436-283">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="6a436-284">Que gera o seguinte HTML (com "CA" selecionada):</span><span class="sxs-lookup"><span data-stu-id="6a436-284">Which generates the following HTML (with "CA" selected):</span></span>

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
> <span data-ttu-id="6a436-285">Não é recomendável usar `ViewBag` ou `ViewData` com o auxiliar de marca selecionar.</span><span class="sxs-lookup"><span data-stu-id="6a436-285">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="6a436-286">Um modelo de exibição é mais robusto fornecer metadados MVC e geralmente menos problemáticos.</span><span class="sxs-lookup"><span data-stu-id="6a436-286">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="6a436-287">O `asp-for` valor de atributo é um caso especial e não requer um `Model` prefixo, os outros não de atributos do auxiliar de marcação (como `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="6a436-287">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="6a436-288">Associação de enum</span><span class="sxs-lookup"><span data-stu-id="6a436-288">Enum binding</span></span>

<span data-ttu-id="6a436-289">Geralmente é conveniente usar `<select>` com um `enum` propriedade e gerar o `SelectListItem` elementos do `enum` valores.</span><span class="sxs-lookup"><span data-stu-id="6a436-289">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="6a436-290">Amostra:</span><span class="sxs-lookup"><span data-stu-id="6a436-290">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="6a436-291">O `GetEnumSelectList` método gera uma `SelectList` objeto para um enum.</span><span class="sxs-lookup"><span data-stu-id="6a436-291">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="6a436-292">Possível decorar o enumerador de lista com o `Display` atributo para obter uma interface de usuário mais rica:</span><span class="sxs-lookup"><span data-stu-id="6a436-292">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="6a436-293">O HTML a seguir é gerado:</span><span class="sxs-lookup"><span data-stu-id="6a436-293">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="6a436-294">Opção de grupo</span><span class="sxs-lookup"><span data-stu-id="6a436-294">Option Group</span></span>

<span data-ttu-id="6a436-295">O HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) elemento é gerado quando o modelo de exibição contém um ou mais `SelectListGroup` objetos.</span><span class="sxs-lookup"><span data-stu-id="6a436-295">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="6a436-296">O `CountryViewModelGroup` grupos a `SelectListItem` elementos nos grupos "América do Norte" e "Europa":</span><span class="sxs-lookup"><span data-stu-id="6a436-296">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="6a436-297">Os dois grupos são mostrados abaixo:</span><span class="sxs-lookup"><span data-stu-id="6a436-297">The two groups are shown below:</span></span>

![exemplo de grupo de opção](working-with-forms/_static/grp.png)

<span data-ttu-id="6a436-299">O código HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="6a436-299">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="6a436-300">Seleção múltipla</span><span class="sxs-lookup"><span data-stu-id="6a436-300">Multiple select</span></span>

<span data-ttu-id="6a436-301">O auxiliar de marca selecione gerará automaticamente o [vários = "várias"](http://w3c.github.io/html-reference/select.html) atributo se a propriedade especificada a `asp-for` atributo é um `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="6a436-301">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="6a436-302">Por exemplo, considerando o seguinte modelo:</span><span class="sxs-lookup"><span data-stu-id="6a436-302">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="6a436-303">Com a seguinte exibição:</span><span class="sxs-lookup"><span data-stu-id="6a436-303">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="6a436-304">Gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="6a436-304">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="6a436-305">Nenhuma seleção</span><span class="sxs-lookup"><span data-stu-id="6a436-305">No selection</span></span>

<span data-ttu-id="6a436-306">Se você estiver usando a opção "não especificada" em várias páginas, você pode criar um modelo para eliminar o HTML de repetição:</span><span class="sxs-lookup"><span data-stu-id="6a436-306">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="6a436-307">O *Views/Shared/EditorTemplates/CountryViewModel.cshtml* modelo:</span><span class="sxs-lookup"><span data-stu-id="6a436-307">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="6a436-308">Adicionando HTML [ \<opção >](https://www.w3.org/wiki/HTML/Elements/option) elementos não está limitado ao *nenhuma seleção* caso.</span><span class="sxs-lookup"><span data-stu-id="6a436-308">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="6a436-309">Por exemplo, o seguinte método de exibição e a ação irá gerar HTML com o código acima:</span><span class="sxs-lookup"><span data-stu-id="6a436-309">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="6a436-310">Corretas `<option>` elemento será selecionado (contêm o `selected="selected"` atributo) dependendo atual `Country` valor.</span><span class="sxs-lookup"><span data-stu-id="6a436-310">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6a436-311">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6a436-311">Additional Resources</span></span>

* [<span data-ttu-id="6a436-312">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="6a436-312">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="6a436-313">Elemento de formulário HTML</span><span class="sxs-lookup"><span data-stu-id="6a436-313">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="6a436-314">Solicitação de Token de verificação</span><span class="sxs-lookup"><span data-stu-id="6a436-314">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="6a436-315">Associação de modelos</span><span class="sxs-lookup"><span data-stu-id="6a436-315">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="6a436-316">Validação de modelo</span><span class="sxs-lookup"><span data-stu-id="6a436-316">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="6a436-317">anotações de dados</span><span class="sxs-lookup"><span data-stu-id="6a436-317">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="6a436-318">[Trechos de código para este documento de código](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span><span class="sxs-lookup"><span data-stu-id="6a436-318">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
