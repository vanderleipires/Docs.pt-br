---
title: "Auxiliar de Marca de Âncora no ASP.NET Core"
author: pkellner
description: "Mostra como trabalhar com o Auxiliar de marca de âncora"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="af94b-103">Auxiliar de marca de âncora</span><span class="sxs-lookup"><span data-stu-id="af94b-103">Anchor Tag Helper</span></span>

<span data-ttu-id="af94b-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="af94b-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="af94b-105">O Auxiliar de marca de âncora aprimora a marca de âncora HTML (`<a ... ></a>`) adicionando novos atributos.</span><span class="sxs-lookup"><span data-stu-id="af94b-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="af94b-106">O link gerado (na marca `href`) é criado usando os novos atributos.</span><span class="sxs-lookup"><span data-stu-id="af94b-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="af94b-107">Essa URL pode incluir um protocolo opcional, como HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="af94b-108">O controlador de alto-falante abaixo é usado nos exemplos deste documento.</span><span class="sxs-lookup"><span data-stu-id="af94b-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="af94b-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="af94b-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="af94b-110">Atributos do Auxiliar de marca de âncora</span><span class="sxs-lookup"><span data-stu-id="af94b-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="af94b-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="af94b-111">asp-controller</span></span>

<span data-ttu-id="af94b-112">`asp-controller` é usado para associar qual controlador será usado para gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="af94b-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="af94b-113">Os controladores especificados devem existir no projeto atual.</span><span class="sxs-lookup"><span data-stu-id="af94b-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="af94b-114">O código a seguir lista todos os alto-falantes:</span><span class="sxs-lookup"><span data-stu-id="af94b-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="af94b-115">A marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="af94b-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="af94b-116">Se o `asp-controller` for especificado e `asp-action` não for, o `asp-action` padrão será o método do controlador padrão da exibição em execução.</span><span class="sxs-lookup"><span data-stu-id="af94b-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="af94b-117">Ou seja, no exemplo acima, se `asp-action` for deixado de fora e este Auxiliar de Marca de Âncora for gerado do (**/Home**) da exibição `Index` de *HomeController*, a marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="af94b-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="af94b-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="af94b-118">asp-action</span></span>

<span data-ttu-id="af94b-119">`asp-action` é o nome do método de ação no controlador que será incluído no `href` gerado.</span><span class="sxs-lookup"><span data-stu-id="af94b-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="af94b-120">Por exemplo, o código a seguir define o `href` gerado para apontar para a página de detalhes do alto-falante:</span><span class="sxs-lookup"><span data-stu-id="af94b-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="af94b-121">A marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="af94b-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="af94b-122">Se nenhum atributo `asp-controller` for especificado, o controlador padrão que está chamando a exibição que executa a exibição atual será usado.</span><span class="sxs-lookup"><span data-stu-id="af94b-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="af94b-123">Se o atributo `asp-action` for `Index`, nenhuma ação será anexada à URL, fazendo com que o método `Index` padrão seja chamado.</span><span class="sxs-lookup"><span data-stu-id="af94b-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="af94b-124">A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="af94b-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="af94b-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="af94b-125">asp-page</span></span>

<span data-ttu-id="af94b-126">Use o atributo `asp-page` em uma marca de âncora para definir sua URL para apontar para uma página específica.</span><span class="sxs-lookup"><span data-stu-id="af94b-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="af94b-127">Prefixar o nome de página com uma barra "/" cria a URL.</span><span class="sxs-lookup"><span data-stu-id="af94b-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="af94b-128">A URL no exemplo abaixo aponta para a página "Speaker" no diretório atual.</span><span class="sxs-lookup"><span data-stu-id="af94b-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="af94b-129">O atributo `asp-page` no exemplo de código anterior renderiza uma saída HTML na exibição semelhante ao trecho a seguir:</span><span class="sxs-lookup"><span data-stu-id="af94b-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="af94b-130">O atributo `asp-page` é mutuamente exclusivo com os atributos `asp-route`, `asp-controller` e `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="af94b-131">No entanto, `asp-page` pode ser usado com `asp-route-id` para controlar o roteamento, como mostra o exemplo de código a seguir:</span><span class="sxs-lookup"><span data-stu-id="af94b-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="af94b-132">O `asp-route-id` produz a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="af94b-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="af94b-133">Para usar o atributo `asp-page` em páginas Razor, as URLs devem ser um caminho relativo, por exemplo, `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="af94b-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="af94b-134">Caminhos relativos no atributo `asp-page` não estão disponíveis em exibições MVC.</span><span class="sxs-lookup"><span data-stu-id="af94b-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="af94b-135">Use a sintaxe "/" para exibições MVC.</span><span class="sxs-lookup"><span data-stu-id="af94b-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="af94b-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="af94b-136">asp-route-{value}</span></span>

<span data-ttu-id="af94b-137">`asp-route-` é um prefixo de rota curinga.</span><span class="sxs-lookup"><span data-stu-id="af94b-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="af94b-138">Qualquer valor colocado após o traço à direita será interpretado como um potencial parâmetro de rota.</span><span class="sxs-lookup"><span data-stu-id="af94b-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="af94b-139">Se uma rota padrão não for encontrada, esse prefixo de rota será anexado ao href gerado como um valor e parâmetro de solicitação.</span><span class="sxs-lookup"><span data-stu-id="af94b-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="af94b-140">Caso contrário, ele será substituído no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="af94b-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="af94b-141">Supondo que você tenha um método de controlador definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="af94b-141">Assuming you have a controller method defined as follows:</span></span>

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

<span data-ttu-id="af94b-142">E que tenha o modelo de rota padrão definido em seu *Startup.cs* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="af94b-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="af94b-143">O arquivo **cshtml** que contém o Auxiliar de marca de âncora necessário para usar o parâmetro de modelo **speaker** passado do controlador para a exibição será o seguinte:</span><span class="sxs-lookup"><span data-stu-id="af94b-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="af94b-144">O código HTML gerado, então, será o seguinte, porque a **id** foi encontrada na rota padrão.</span><span class="sxs-lookup"><span data-stu-id="af94b-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="af94b-145">Se o prefixo da rota não fizer parte do modelo de roteamento encontrado, que é o caso com o seguinte arquivo **cshtml**:</span><span class="sxs-lookup"><span data-stu-id="af94b-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="af94b-146">O código HTML gerado, então, será o seguinte, porque **speakerid** não foi encontrado na rota correspondente:</span><span class="sxs-lookup"><span data-stu-id="af94b-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="af94b-147">Se `asp-controller` ou `asp-action` não for especificado, o mesmo processamento padrão será seguido, como no atributo `asp-route`.</span><span class="sxs-lookup"><span data-stu-id="af94b-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="af94b-148">asp-route</span><span class="sxs-lookup"><span data-stu-id="af94b-148">asp-route</span></span>

<span data-ttu-id="af94b-149">`asp-route` possibilita criar uma URL que leva diretamente a uma rota nomeada.</span><span class="sxs-lookup"><span data-stu-id="af94b-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="af94b-150">Usando atributos de roteamento, uma rota pode ser nomeada como mostrado em `SpeakerController` e usada em seu método `Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="af94b-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="af94b-151">`Name = "speakerevals"` instrui o Auxiliar de Marca de Âncora a gerar uma rota diretamente para esse método de controlador usando a URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="af94b-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="af94b-152">Se `asp-controller` ou `asp-action` for especificado além de `asp-route`, a rota gerada poderá não ser a esperada.</span><span class="sxs-lookup"><span data-stu-id="af94b-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="af94b-153">`asp-route` não deve ser usado com os atributos `asp-controller` ou `asp-action` para evitar um conflito de rota.</span><span class="sxs-lookup"><span data-stu-id="af94b-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="af94b-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="af94b-154">asp-all-route-data</span></span>

<span data-ttu-id="af94b-155">`asp-all-route-data` permite criar um dicionário de pares chave-valor, em que a chave é o nome do parâmetro e o valor é o valor associado a essa chave.</span><span class="sxs-lookup"><span data-stu-id="af94b-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="af94b-156">Como o exemplo a seguir mostra, um dicionário embutido é criado e os dados são passados para a exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="af94b-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="af94b-157">Como alternativa, os dados tambám poderiam ser passados com seu modelo.</span><span class="sxs-lookup"><span data-stu-id="af94b-157">As an alternative, the data could also be passed in with your model.</span></span>

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

<span data-ttu-id="af94b-158">O código anterior gera a seguinte URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="af94b-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="af94b-159">Quando o link é acionado, o método do controlador `EvaluationsCurrent` é chamado.</span><span class="sxs-lookup"><span data-stu-id="af94b-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="af94b-160">Ele é chamado porque esse controlador tem dois parâmetros de cadeia de caracteres que correspondem ao que foi criado do dicionário `asp-all-route-data`.</span><span class="sxs-lookup"><span data-stu-id="af94b-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="af94b-161">Se alguma chave no dicionário for correspondente aos parâmetros da rota, esses valores serão substituídos na rota conforme apropriado e os outros valores não correspondentes serão gerados como parâmetros de solicitação.</span><span class="sxs-lookup"><span data-stu-id="af94b-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="af94b-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="af94b-162">asp-fragment</span></span>

<span data-ttu-id="af94b-163">`asp-fragment` define um fragmento de URL para ser acrescentado à URL.</span><span class="sxs-lookup"><span data-stu-id="af94b-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="af94b-164">O Auxiliar de marca de âncora adicionará o caractere de hash (#).</span><span class="sxs-lookup"><span data-stu-id="af94b-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="af94b-165">Se você criar uma marca:</span><span class="sxs-lookup"><span data-stu-id="af94b-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="af94b-166">A URL gerada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="af94b-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="af94b-167">Marcas de hash são úteis ao criar aplicativos do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="af94b-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="af94b-168">Elas podem ser usadas para marcar e pesquisar com facilidade em JavaScript, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="af94b-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="af94b-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="af94b-169">asp-area</span></span>

<span data-ttu-id="af94b-170">`asp-area` define o nome da área que o ASP.NET Core usa para definir a rota apropriada.</span><span class="sxs-lookup"><span data-stu-id="af94b-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="af94b-171">A seguir, há exemplos de como o atributo de área causa um remapeamento de rotas.</span><span class="sxs-lookup"><span data-stu-id="af94b-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="af94b-172">Definir `asp-area` como "Blogs" faz com que o diretório `Areas/Blogs` seja prefixado nas rotas dos controladores e exibições associados a essa marca de âncora.</span><span class="sxs-lookup"><span data-stu-id="af94b-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="af94b-173">Nome do projeto</span><span class="sxs-lookup"><span data-stu-id="af94b-173">Project name</span></span>
  * <span data-ttu-id="af94b-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="af94b-174">wwwroot</span></span>
  * <span data-ttu-id="af94b-175">Áreas</span><span class="sxs-lookup"><span data-stu-id="af94b-175">Areas</span></span>
    * <span data-ttu-id="af94b-176">Blogs</span><span class="sxs-lookup"><span data-stu-id="af94b-176">Blogs</span></span>
      * <span data-ttu-id="af94b-177">Controladores</span><span class="sxs-lookup"><span data-stu-id="af94b-177">Controllers</span></span>
        * <span data-ttu-id="af94b-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="af94b-178">HomeController.cs</span></span>
      * <span data-ttu-id="af94b-179">Exibições</span><span class="sxs-lookup"><span data-stu-id="af94b-179">Views</span></span>
        * <span data-ttu-id="af94b-180">Home</span><span class="sxs-lookup"><span data-stu-id="af94b-180">Home</span></span>
          * <span data-ttu-id="af94b-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="af94b-181">Index.cshtml</span></span>
          * <span data-ttu-id="af94b-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="af94b-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="af94b-183">Controladores</span><span class="sxs-lookup"><span data-stu-id="af94b-183">Controllers</span></span>

<span data-ttu-id="af94b-184">Especificar uma marca de área válida, como ```area="Blogs"```, ao referenciar o arquivo ```AboutBlog.cshtml``` será semelhante ao seguinte quando o Auxiliar de marca de âncora estiver sendo usado.</span><span class="sxs-lookup"><span data-stu-id="af94b-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="af94b-185">O código HTML gerado incluirá o segmento de áreas e será o seguinte:</span><span class="sxs-lookup"><span data-stu-id="af94b-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="af94b-186">Para que áreas do MVC funcionem em um aplicativo Web, o modelo de rota deve incluir uma referência à área, se ela existir.</span><span class="sxs-lookup"><span data-stu-id="af94b-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="af94b-187">Esse modelo, que é o segundo parâmetro da chamada de método `routes.MapRoute`, será exibido como: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="af94b-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="af94b-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="af94b-188">asp-protocol</span></span>

<span data-ttu-id="af94b-189">O `asp-protocol` é destinado a especificar um protocolo (como `https`) em sua URL.</span><span class="sxs-lookup"><span data-stu-id="af94b-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="af94b-190">Um exemplo de Auxiliar de marca de âncora que incluir o protocolo será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="af94b-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="af94b-191">e gerará o seguinte HTML:</span><span class="sxs-lookup"><span data-stu-id="af94b-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="af94b-192">O domínio no exemplo é localhost, mas o Auxiliar de marca de âncora usará o domínio público do site ao gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="af94b-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af94b-193">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="af94b-193">Additional resources</span></span>

* [<span data-ttu-id="af94b-194">Áreas</span><span class="sxs-lookup"><span data-stu-id="af94b-194">Areas</span></span>](xref:mvc/controllers/areas)
