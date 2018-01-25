---
title: Auxiliar de marca de ancoragem | Microsoft Docs
author: pkellner
description: "Mostra como trabalhar com o auxiliar de marca de âncora"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 74609b515936ec7da8bfc133c27cb69f51311924
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="2478f-103">Auxiliar de marca de âncora</span><span class="sxs-lookup"><span data-stu-id="2478f-103">Anchor Tag Helper</span></span>

<span data-ttu-id="2478f-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2478f-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="2478f-105">O auxiliar de marca de âncora aprimora a âncora HTML (`<a ... ></a>`) marca adicionando novos atributos.</span><span class="sxs-lookup"><span data-stu-id="2478f-105">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="2478f-106">O link gerado (no `href` marca) é criado usando os novos atributos.</span><span class="sxs-lookup"><span data-stu-id="2478f-106">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="2478f-107">Essa URL pode incluir um protocolo opcional, como HTTP.</span><span class="sxs-lookup"><span data-stu-id="2478f-107">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="2478f-108">O controlador do apresentador abaixo é usado nos exemplos neste documento.</span><span class="sxs-lookup"><span data-stu-id="2478f-108">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="2478f-109">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="2478f-109">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="2478f-110">Atributos de auxiliar de marca de âncora</span><span class="sxs-lookup"><span data-stu-id="2478f-110">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="2478f-111">asp-controller</span><span class="sxs-lookup"><span data-stu-id="2478f-111">asp-controller</span></span>

<span data-ttu-id="2478f-112">`asp-controller`é usado para associar o controlador será usado para gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="2478f-112">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="2478f-113">Os controladores especificados devem existir no projeto atual.</span><span class="sxs-lookup"><span data-stu-id="2478f-113">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="2478f-114">O código a seguir lista todos os alto-falantes:</span><span class="sxs-lookup"><span data-stu-id="2478f-114">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="2478f-115">A marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="2478f-115">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="2478f-116">Se o `asp-controller` for especificado e `asp-action` não estiver, o padrão `asp-action` será o método do controlador padrão do modo de exibição atualmente em execução.</span><span class="sxs-lookup"><span data-stu-id="2478f-116">If the `asp-controller` is specified and `asp-action` isn't, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="2478f-117">Que é, no exemplo acima, se `asp-action` é à esquerda, e este auxiliar de marca de âncora é gerado a partir *HomeController*do `Index` exibição (**/home**), a marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="2478f-117">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="2478f-118">asp-action</span><span class="sxs-lookup"><span data-stu-id="2478f-118">asp-action</span></span>

<span data-ttu-id="2478f-119">`asp-action`é o nome do método de ação no controlador que serão incluído no gerado `href`.</span><span class="sxs-lookup"><span data-stu-id="2478f-119">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="2478f-120">Por exemplo, o código a seguir defina gerado `href` para apontar para a página de detalhes do apresentador:</span><span class="sxs-lookup"><span data-stu-id="2478f-120">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="2478f-121">A marcação gerada será:</span><span class="sxs-lookup"><span data-stu-id="2478f-121">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="2478f-122">Se nenhum `asp-controller` atributo for especificado, o controlador padrão chamando o modo de exibição que executar a exibição atual será usado.</span><span class="sxs-lookup"><span data-stu-id="2478f-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="2478f-123">Se o atributo `asp-action` é `Index`, em seguida, nenhuma ação é anexada à URL, à esquerda para o padrão `Index` método ser chamado.</span><span class="sxs-lookup"><span data-stu-id="2478f-123">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="2478f-124">A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="2478f-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="2478f-125">asp-page</span><span class="sxs-lookup"><span data-stu-id="2478f-125">asp-page</span></span>

<span data-ttu-id="2478f-126">Use o `asp-page` atributo em uma marca de âncora para definir a URL para apontar para uma página específica.</span><span class="sxs-lookup"><span data-stu-id="2478f-126">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="2478f-127">Prefixando o nome de página com uma barra "/" cria a URL.</span><span class="sxs-lookup"><span data-stu-id="2478f-127">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="2478f-128">A URL de exemplo abaixo aponta para a página "Locutor" no diretório atual.</span><span class="sxs-lookup"><span data-stu-id="2478f-128">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="2478f-129">O `asp-page` atributo no exemplo de código anterior renderiza a saída HTML no modo de exibição semelhante para o trecho a seguir:</span><span class="sxs-lookup"><span data-stu-id="2478f-129">The `asp-page` attribute in the previous code sample renders HTML output in the view that's similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

<span data-ttu-id="2478f-130">O `asp-page` atributo é mutuamente exclusivo com o `asp-route`, `asp-controller`, e `asp-action` atributos.</span><span class="sxs-lookup"><span data-stu-id="2478f-130">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="2478f-131">No entanto, `asp-page` pode ser usado com `asp-route-id` para controlar o roteamento, como mostra o exemplo de código a seguir:</span><span class="sxs-lookup"><span data-stu-id="2478f-131">However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:</span></span>

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="2478f-132">O `asp-route-id` produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="2478f-132">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="2478f-133">Para usar o `asp-page` atributo nas páginas Razor, as URLs deve ser um caminho relativo, por exemplo `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="2478f-133">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="2478f-134">Caminhos relativos no `asp-page` atributo não estão disponíveis nos modos de exibição do MVC.</span><span class="sxs-lookup"><span data-stu-id="2478f-134">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="2478f-135">Use a sintaxe "/" para exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="2478f-135">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="2478f-136">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="2478f-136">asp-route-{value}</span></span>

<span data-ttu-id="2478f-137">`asp-route-`é um prefixo de rota de curinga.</span><span class="sxs-lookup"><span data-stu-id="2478f-137">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="2478f-138">Qualquer valor colocado após o traço à direita será interpretado como um parâmetro de rota potencial.</span><span class="sxs-lookup"><span data-stu-id="2478f-138">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="2478f-139">Se não for encontrada uma rota padrão, esse prefixo de rota será anexado para o href gerado como um parâmetro de solicitação e um valor.</span><span class="sxs-lookup"><span data-stu-id="2478f-139">If a default route isn't found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="2478f-140">Caso contrário, ele será substituído no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="2478f-140">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="2478f-141">Supondo que você tenha um método de controlador definido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2478f-141">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="2478f-142">E que o modelo de rota padrão definido no seu *Startup.cs* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2478f-142">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="2478f-143">O **cshtml** arquivo que contém o auxiliar de marca de âncora necessário usar o **locutor** parâmetro de modelo passado do controlador para o modo de exibição é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2478f-143">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="2478f-144">O código HTML gerado será da seguinte maneira porque **id** foi encontrado na rota padrão.</span><span class="sxs-lookup"><span data-stu-id="2478f-144">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="2478f-145">Se o prefixo da rota não faz parte do modelo de roteamento encontrado, que é o caso com o seguinte **cshtml** arquivo:</span><span class="sxs-lookup"><span data-stu-id="2478f-145">If the route prefix isn't part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="2478f-146">O código HTML gerado será da seguinte maneira porque **speakerid** não foi encontrado na rota correspondida:</span><span class="sxs-lookup"><span data-stu-id="2478f-146">The generated HTML will then be as follows because **speakerid** wasn't found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="2478f-147">Se qualquer um dos `asp-controller` ou `asp-action` não forem especificados, e o processamento padrão mesmo é seguido porque está sendo o `asp-route` atributo.</span><span class="sxs-lookup"><span data-stu-id="2478f-147">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="2478f-148">rota de ASP</span><span class="sxs-lookup"><span data-stu-id="2478f-148">asp-route</span></span>

<span data-ttu-id="2478f-149">`asp-route`Fornece uma maneira de criar uma URL que vincula-se diretamente a uma rota nomeada.</span><span class="sxs-lookup"><span data-stu-id="2478f-149">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="2478f-150">Usando atributos de roteamento, uma rota pode ser nomeada como mostra o `SpeakerController` e usado em seu `Evaluations` método.</span><span class="sxs-lookup"><span data-stu-id="2478f-150">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="2478f-151">`Name = "speakerevals"`informa o auxiliar de marca de âncora para gerar uma rota diretamente para esse método de controlador usando a URL `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="2478f-151">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="2478f-152">Se `asp-controller` ou `asp-action` é especificado além `asp-route`, a rota gerada não pode ser o esperado.</span><span class="sxs-lookup"><span data-stu-id="2478f-152">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="2478f-153">`asp-route`não deve ser usado com qualquer um dos atributos `asp-controller` ou `asp-action` para evitar um conflito de rota.</span><span class="sxs-lookup"><span data-stu-id="2478f-153">`asp-route` shouldn't be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="2478f-154">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="2478f-154">asp-all-route-data</span></span>

<span data-ttu-id="2478f-155">`asp-all-route-data`permite a criação de um dicionário de pares chave-valor em que a chave é o nome do parâmetro e o valor é o valor associado a essa chave.</span><span class="sxs-lookup"><span data-stu-id="2478f-155">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="2478f-156">Como o exemplo a seguir mostra, um dicionário embutido é criado e os dados são passados para o modo de exibição do razor.</span><span class="sxs-lookup"><span data-stu-id="2478f-156">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="2478f-157">Como alternativa, os dados podem ser passados com seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2478f-157">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="2478f-158">O código anterior gera a seguinte URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="2478f-158">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="2478f-159">Quando o link é clicado, o método do controlador `EvaluationsCurrent` é chamado.</span><span class="sxs-lookup"><span data-stu-id="2478f-159">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="2478f-160">Ele é chamado porque esse controlador tem dois parâmetros de cadeia de caracteres que correspondem o que foi criado a partir de `asp-all-route-data` dicionário.</span><span class="sxs-lookup"><span data-stu-id="2478f-160">It's called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="2478f-161">Se todas as chaves na correspondência de dicionário de parâmetros de rota, esses valores são substituídos na rota conforme apropriado e os outros valores não correspondentes serão gerados como parâmetros de solicitação.</span><span class="sxs-lookup"><span data-stu-id="2478f-161">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="2478f-162">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="2478f-162">asp-fragment</span></span>

<span data-ttu-id="2478f-163">`asp-fragment`define um fragmento de URL para acrescentar à URL.</span><span class="sxs-lookup"><span data-stu-id="2478f-163">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="2478f-164">O auxiliar de marca de âncora adicionará o caractere de hash (#).</span><span class="sxs-lookup"><span data-stu-id="2478f-164">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="2478f-165">Se você criar uma marca:</span><span class="sxs-lookup"><span data-stu-id="2478f-165">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="2478f-166">A URL gerada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="2478f-166">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="2478f-167">Marcas de hash são úteis ao criar aplicativos do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="2478f-167">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="2478f-168">Eles podem ser usados para marcar fácil e pesquisa em JavaScript, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="2478f-168">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="2478f-169">asp-area</span><span class="sxs-lookup"><span data-stu-id="2478f-169">asp-area</span></span>

<span data-ttu-id="2478f-170">`asp-area`Define o nome da área que usa o ASP.NET Core para definir a rota apropriada.</span><span class="sxs-lookup"><span data-stu-id="2478f-170">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="2478f-171">A seguir estão exemplos de como o atributo área faz com que um remapeamento de rotas.</span><span class="sxs-lookup"><span data-stu-id="2478f-171">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="2478f-172">Configuração `asp-area` blogs prefixos de diretório `Areas/Blogs` para as rotas do associado controladores e exibições para a marca de âncora.</span><span class="sxs-lookup"><span data-stu-id="2478f-172">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="2478f-173">Nome do projeto</span><span class="sxs-lookup"><span data-stu-id="2478f-173">Project name</span></span>
  * <span data-ttu-id="2478f-174">wwwroot</span><span class="sxs-lookup"><span data-stu-id="2478f-174">wwwroot</span></span>
  * <span data-ttu-id="2478f-175">Áreas</span><span class="sxs-lookup"><span data-stu-id="2478f-175">Areas</span></span>
    * <span data-ttu-id="2478f-176">Blogs</span><span class="sxs-lookup"><span data-stu-id="2478f-176">Blogs</span></span>
      * <span data-ttu-id="2478f-177">Controladores</span><span class="sxs-lookup"><span data-stu-id="2478f-177">Controllers</span></span>
        * <span data-ttu-id="2478f-178">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="2478f-178">HomeController.cs</span></span>
      * <span data-ttu-id="2478f-179">Exibições</span><span class="sxs-lookup"><span data-stu-id="2478f-179">Views</span></span>
        * <span data-ttu-id="2478f-180">Home</span><span class="sxs-lookup"><span data-stu-id="2478f-180">Home</span></span>
          * <span data-ttu-id="2478f-181">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2478f-181">Index.cshtml</span></span>
          * <span data-ttu-id="2478f-182">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="2478f-182">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="2478f-183">Controladores</span><span class="sxs-lookup"><span data-stu-id="2478f-183">Controllers</span></span>

<span data-ttu-id="2478f-184">Especificando uma marca de área é válida, como ```area="Blogs"``` ao referenciar o ```AboutBlog.cshtml``` arquivo será semelhante a seguir usando o auxiliar de marca de âncora.</span><span class="sxs-lookup"><span data-stu-id="2478f-184">Specifying an area tag that's valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="2478f-185">O código HTML gerado incluirá o segmento de áreas e será da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2478f-185">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="2478f-186">Para as áreas do MVC trabalhar em um aplicativo web, o modelo de rota deve incluir uma referência para a área se ele existir.</span><span class="sxs-lookup"><span data-stu-id="2478f-186">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="2478f-187">Esse modelo, que é o segundo parâmetro do `routes.MapRoute` chamada de método, será exibida como:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="2478f-187">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="2478f-188">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="2478f-188">asp-protocol</span></span>

<span data-ttu-id="2478f-189">O `asp-protocol` é para especificar um protocolo (como `https`) em sua URL.</span><span class="sxs-lookup"><span data-stu-id="2478f-189">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="2478f-190">Um auxiliar de marca de âncora que contenha o protocolo de exemplo será semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="2478f-190">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="2478f-191">e irá gerar o HTML da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2478f-191">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="2478f-192">O domínio no exemplo é localhost, mas o auxiliar de marca de âncora usará o domínio do site público ao gerar a URL.</span><span class="sxs-lookup"><span data-stu-id="2478f-192">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2478f-193">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2478f-193">Additional resources</span></span>

* [<span data-ttu-id="2478f-194">Áreas</span><span class="sxs-lookup"><span data-stu-id="2478f-194">Areas</span></span>](xref:mvc/controllers/areas)
