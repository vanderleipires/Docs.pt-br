---
title: Auxiliar de Marca de Âncora no ASP.NET Core
author: pkellner
description: Descubra os atributos do Auxiliar de Marca de Âncora do ASP.NET e a função que cada atributo desempenha no comportamento de extensão da marca de âncora de HTML.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 31ff62b6bedb5e577a51f341c89d241d06a83ad3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="619fa-103">Auxiliar de Marca de Âncora no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="619fa-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="619fa-104">De [Peter Kellner](http://peterkellner.net) e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="619fa-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="619fa-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="619fa-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="619fa-106">O [Auxiliar de Marca de Âncora](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) aprimora a marca de âncora HTML padrão (`<a ... ></a>`) adicionando novos atributos.</span><span class="sxs-lookup"><span data-stu-id="619fa-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="619fa-107">Por convenção, os nomes de atributos são prefixados com `asp-`.</span><span class="sxs-lookup"><span data-stu-id="619fa-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="619fa-108">O valor do atributo `href` do elemento de âncora renderizado é determinado pelos valores dos atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="619fa-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="619fa-109">*SpeakerController* é usado em exemplos ao longo de todo este documento:</span><span class="sxs-lookup"><span data-stu-id="619fa-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="619fa-110">Segue um inventário dos atributos `asp-`.</span><span class="sxs-lookup"><span data-stu-id="619fa-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="619fa-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="619fa-111">asp-controller</span></span>

<span data-ttu-id="619fa-112">O atributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) designa o controlador usado para gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="619fa-113">A marcação a seguir lista todos os palestrantes:</span><span class="sxs-lookup"><span data-stu-id="619fa-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="619fa-114">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="619fa-115">Se o atributo `asp-controller` for especificado e `asp-action` não for, o valor `asp-action` padrão é a ação do controlador associada ao modo de exibição em execução no momento.</span><span class="sxs-lookup"><span data-stu-id="619fa-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="619fa-116">Se `asp-action` for omitido da marcação anterior e o Auxiliar de Marca de Âncora for usado no modo de exibição *Índice* (*/home*) do *HomeController*, o HTML gerado será:</span><span class="sxs-lookup"><span data-stu-id="619fa-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="619fa-117">asp-action</span><span class="sxs-lookup"><span data-stu-id="619fa-117">asp-action</span></span>

<span data-ttu-id="619fa-118">O valor do atributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) representa o nome da ação do controlador incluído no atributo `href` gerado.</span><span class="sxs-lookup"><span data-stu-id="619fa-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="619fa-119">A seguinte marcação define o valor do atributo `href` gerado para a página de avaliações do palestrante:</span><span class="sxs-lookup"><span data-stu-id="619fa-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="619fa-120">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="619fa-121">Se nenhum atributo `asp-controller` for especificado, o controlador padrão que está chamando a exibição que executa a exibição atual é usado.</span><span class="sxs-lookup"><span data-stu-id="619fa-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="619fa-122">Se o valor do atributo `asp-action` for `Index`, nenhuma ação será anexada à URL, causando a invocação da ação `Index` padrão.</span><span class="sxs-lookup"><span data-stu-id="619fa-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="619fa-123">A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="619fa-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="619fa-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="619fa-124">asp-route-{value}</span></span>

<span data-ttu-id="619fa-125">O atributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite um prefixo de roteamento de caractere curinga.</span><span class="sxs-lookup"><span data-stu-id="619fa-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="619fa-126">Qualquer valor que esteja ocupando o espaço reservado `{value}` é interpretado como um possível parâmetro de roteamento.</span><span class="sxs-lookup"><span data-stu-id="619fa-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="619fa-127">Se uma rota padrão não for encontrada, esse prefixo de rota será anexado ao atributo `href` gerado como um valor e parâmetro de solicitação.</span><span class="sxs-lookup"><span data-stu-id="619fa-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="619fa-128">Caso contrário, ele será substituído no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="619fa-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="619fa-129">Considere a seguinte ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="619fa-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="619fa-130">Com um modelo de rota padrão definido em *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="619fa-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="619fa-131">O modo de exibição MVC usa o modelo fornecido pela ação, da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="619fa-131">The MVC view uses the model, provided by the action, as follows:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

<span data-ttu-id="619fa-132">O espaço reservado `{id?}` da rota padrão foi correspondido.</span><span class="sxs-lookup"><span data-stu-id="619fa-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="619fa-133">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="619fa-134">Suponha que o prefixo de roteamento não faça parte do modelo de roteamento de correspondência, como com o seguinte modo de exibição de MVC:</span><span class="sxs-lookup"><span data-stu-id="619fa-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

<span data-ttu-id="619fa-135">O seguinte HTML é gerado porque o `speakerid` não foi encontrado na rota correspondente:</span><span class="sxs-lookup"><span data-stu-id="619fa-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="619fa-136">Se `asp-controller` ou `asp-action` não forem especificados, o mesmo processamento padrão será seguido, como no atributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="619fa-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="619fa-137">asp-route</span><span class="sxs-lookup"><span data-stu-id="619fa-137">asp-route</span></span>

<span data-ttu-id="619fa-138">O atributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) é usado para criar um link de URL diretamente para uma rota nomeada.</span><span class="sxs-lookup"><span data-stu-id="619fa-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="619fa-139">Usando [atributos de roteamento](xref:mvc/controllers/routing#attribute-routing), uma rota pode ser nomeada como mostrado em `SpeakerController` e usada em sua ação `Evaluations`:</span><span class="sxs-lookup"><span data-stu-id="619fa-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="619fa-140">Na seguinte marcação, o atributo `asp-route` faz referência à rota nomeada:</span><span class="sxs-lookup"><span data-stu-id="619fa-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="619fa-141">O Auxiliar de Marca de Âncora gera uma rota diretamente para essa ação de controlador usando a URL */Speaker/Evaluations*.</span><span class="sxs-lookup"><span data-stu-id="619fa-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="619fa-142">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="619fa-143">Se `asp-controller` ou `asp-action` for especificado além de `asp-route`, a rota gerada poderá não ser a esperada.</span><span class="sxs-lookup"><span data-stu-id="619fa-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="619fa-144">Para evitar um conflito de rota, `asp-route` não deve ser usado com os atributos `asp-controller` ou `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="619fa-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="619fa-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="619fa-145">asp-all-route-data</span></span>

<span data-ttu-id="619fa-146">O atributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) oferece suporte à criação de um dicionário ou par chave-valor.</span><span class="sxs-lookup"><span data-stu-id="619fa-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="619fa-147">A chave é o nome do parâmetro e o valor é o valor do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="619fa-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="619fa-148">No exemplo a seguir, um dicionário é inicializado e passado para um modo de exibição Razor.</span><span class="sxs-lookup"><span data-stu-id="619fa-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="619fa-149">Como alternativa, os dados podem ser passado com seu modelo.</span><span class="sxs-lookup"><span data-stu-id="619fa-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="619fa-150">O código anterior gera o seguinte HTML:</span><span class="sxs-lookup"><span data-stu-id="619fa-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="619fa-151">O dicionário `asp-all-route-data` é simplificado para produzir um querystring que atenda aos requisitos da ação `Evaluations` sobrecarregada:</span><span class="sxs-lookup"><span data-stu-id="619fa-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="619fa-152">Se todas as chaves no dicionário corresponderem aos parâmetros, esses valores serão substituídos na rota conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="619fa-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="619fa-153">Os outros valores não correspondentes são gerados como parâmetros de solicitação.</span><span class="sxs-lookup"><span data-stu-id="619fa-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="619fa-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="619fa-154">asp-fragment</span></span>

<span data-ttu-id="619fa-155">O atributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) define um fragmento de URL para anexar à URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="619fa-156">O Auxiliar de Marca de Âncora adiciona o caractere de hash (#).</span><span class="sxs-lookup"><span data-stu-id="619fa-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="619fa-157">Considere a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="619fa-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="619fa-158">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="619fa-159">As marcas de hash são úteis ao criar aplicativos do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="619fa-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="619fa-160">Elas podem ser usadas para marcar e pesquisar com facilidade em JavaScript, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="619fa-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="619fa-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="619fa-161">asp-area</span></span>

<span data-ttu-id="619fa-162">O atributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) define o nome de área usado para definir a rota apropriada.</span><span class="sxs-lookup"><span data-stu-id="619fa-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="619fa-163">O exemplo a seguir ilustra como o atributo de área causa um remapeamento das rotas.</span><span class="sxs-lookup"><span data-stu-id="619fa-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="619fa-164">Definir `asp-area` como "Blogs" faz com que o diretório *Áreas/Blogs* seja prefixado nas rotas dos controladores e exibições associados a essa marca de âncora.</span><span class="sxs-lookup"><span data-stu-id="619fa-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="619fa-165">**<Nome do projeto\>**</span><span class="sxs-lookup"><span data-stu-id="619fa-165">**<Project name\>**</span></span>
  * <span data-ttu-id="619fa-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="619fa-166">**wwwroot**</span></span>
  * <span data-ttu-id="619fa-167">**Áreas**</span><span class="sxs-lookup"><span data-stu-id="619fa-167">**Areas**</span></span>
    * <span data-ttu-id="619fa-168">**Blogs**</span><span class="sxs-lookup"><span data-stu-id="619fa-168">**Blogs**</span></span>
      * <span data-ttu-id="619fa-169">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="619fa-169">**Controllers**</span></span>
        * <span data-ttu-id="619fa-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="619fa-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="619fa-171">**Exibições**</span><span class="sxs-lookup"><span data-stu-id="619fa-171">**Views**</span></span>
        * <span data-ttu-id="619fa-172">**Início**</span><span class="sxs-lookup"><span data-stu-id="619fa-172">**Home**</span></span>
          * <span data-ttu-id="619fa-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="619fa-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="619fa-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="619fa-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="619fa-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="619fa-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="619fa-176">**Controladores**</span><span class="sxs-lookup"><span data-stu-id="619fa-176">**Controllers**</span></span>

<span data-ttu-id="619fa-177">Dada a hierarquia do diretório anterior, a marcação para referenciar o arquivo *AboutBlog.cshtml* é:</span><span class="sxs-lookup"><span data-stu-id="619fa-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="619fa-178">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="619fa-179">Para que as áreas funcionem em um aplicativo MVC, o modelo de rota deve incluir uma referência à área, se ela existir.</span><span class="sxs-lookup"><span data-stu-id="619fa-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="619fa-180">Esse modelo é representado pelo segundo parâmetro da chamada do método `routes.MapRoute` em *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="619fa-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="619fa-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="619fa-181">asp-protocol</span></span>

<span data-ttu-id="619fa-182">O atributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) é destinado a especificar um protocolo (como `https`) em sua URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="619fa-183">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="619fa-183">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="619fa-184">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-184">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="619fa-185">O nome do host no exemplo é localhost, mas o Auxiliar de Marca de Âncora usará o domínio público do site ao gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-185">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="619fa-186">asp-host</span><span class="sxs-lookup"><span data-stu-id="619fa-186">asp-host</span></span>

<span data-ttu-id="619fa-187">O atributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) é para especificar um nome do host na sua URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-187">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="619fa-188">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="619fa-188">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="619fa-189">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-189">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="619fa-190">asp-page</span><span class="sxs-lookup"><span data-stu-id="619fa-190">asp-page</span></span>

<span data-ttu-id="619fa-191">O atributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) é usado com as Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="619fa-191">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="619fa-192">Use-o para definir um valor de atributo `href` da marca de âncora para uma página específica.</span><span class="sxs-lookup"><span data-stu-id="619fa-192">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="619fa-193">Prefixar o nome de página com uma barra ("/") cria a URL.</span><span class="sxs-lookup"><span data-stu-id="619fa-193">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="619fa-194">O exemplo a seguir aponta para o participante da Página Razor:</span><span class="sxs-lookup"><span data-stu-id="619fa-194">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="619fa-195">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-195">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="619fa-196">O atributo `asp-page` é mutuamente exclusivo com os atributos `asp-route`, `asp-controller` e `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="619fa-196">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="619fa-197">No entanto, o `asp-page` pode ser usado com o `asp-route-{value}` para controlar o roteamento, como mostra a marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="619fa-197">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="619fa-198">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-198">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="619fa-199">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="619fa-199">asp-page-handler</span></span>

<span data-ttu-id="619fa-200">O atributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) é usado com as Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="619fa-200">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="619fa-201">Ele se destina à vinculação a manipuladores de página específicos.</span><span class="sxs-lookup"><span data-stu-id="619fa-201">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="619fa-202">Considere o seguinte manipulador de página:</span><span class="sxs-lookup"><span data-stu-id="619fa-202">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="619fa-203">A marcação associada do modelo de página vincula ao manipulador de página `OnGetProfile`.</span><span class="sxs-lookup"><span data-stu-id="619fa-203">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="619fa-204">Observe que o prefixo `On<Verb>` do nome do método do manipulador de página é omitido no valor do atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="619fa-204">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="619fa-205">Se esse fosse um método assíncrono, o sufixo `Async` também seria omitido.</span><span class="sxs-lookup"><span data-stu-id="619fa-205">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="619fa-206">O HTML gerado:</span><span class="sxs-lookup"><span data-stu-id="619fa-206">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="619fa-207">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="619fa-207">Additional resources</span></span>

* [<span data-ttu-id="619fa-208">Áreas</span><span class="sxs-lookup"><span data-stu-id="619fa-208">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="619fa-209">Introdução às Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="619fa-209">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
