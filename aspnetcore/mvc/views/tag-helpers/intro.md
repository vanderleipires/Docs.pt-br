---
title: "Auxiliares de marcação no núcleo do ASP.NET"
author: rick-anderson
description: "Saiba quais são os auxiliares de marcação e como usá-los em ASP.NET Core."
keywords: "ASP.NET Core, auxiliares de marcação"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06644b8359fb5ccc2e61a17a4c6e20e354d5ceef
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="a6b1e-104">Introdução ao auxiliares de marcação no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6b1e-104">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="a6b1e-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6b1e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="a6b1e-106">Quais são os auxiliares de marcação?</span><span class="sxs-lookup"><span data-stu-id="a6b1e-106">What are Tag Helpers?</span></span>

<span data-ttu-id="a6b1e-107">Auxiliares de marcação permitem que o código do lado do servidor participar de criação e renderização de elementos HTML em arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-107">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="a6b1e-108">Por exemplo, o interno `ImageTagHelper` pode acrescentar um número de versão para o nome da imagem.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-108">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="a6b1e-109">Sempre que a imagem é alterada, o servidor gera uma nova versão exclusiva para a imagem, para que os clientes têm garantia de obter a imagem atual (em vez de uma imagem em cache obsoleta).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-109">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="a6b1e-110">Há muitos auxiliares de marcação interna para tarefas comuns - como a criação de formulários, links, ativos de carregamento e pacotes mais - e ainda mais disponíveis em repositórios GitHub públicos e como NuGet.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-110">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="a6b1e-111">Auxiliares de marca são criados no c#, e eles se destinam a elementos HTML com base no nome do elemento, o nome do atributo ou marca pai.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-111">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="a6b1e-112">Por exemplo, o interno `LabelTagHelper` pode direcionar o HTML `<label>` elemento quando o `LabelTagHelper` atributos são aplicados.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-112">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="a6b1e-113">Se você estiver familiarizado com [auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), auxiliares de marcação reduzir as transições explícitas entre HTML e c# em modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-113">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="a6b1e-114">Em muitos casos, auxiliares HTML fornecem uma abordagem alternativa para um auxiliar de marca específica, mas é importante reconhecer que auxiliares de marcação não substituem auxiliares HTML e não é um auxiliar de marca para cada auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-114">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="a6b1e-115">[Em comparação comparados auxiliares HTML de auxiliares de marcação](#tag-helpers-compared-to-html-helpers) explica as diferenças em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-115">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="a6b1e-116">O que fornece os auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="a6b1e-116">What Tag Helpers provide</span></span>

<span data-ttu-id="a6b1e-117">**Uma experiência de desenvolvimento compatível com HTML** para a maior parte, marcação Razor usando auxiliares de marcação parece HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-117">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="a6b1e-118">Designers de front-end familiarizadas com HTML/CSS/JavaScript podem editar Razor sem aprender a sintaxe do Razor c#.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-118">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="a6b1e-119">**Um ambiente rico de IntelliSense para criar marcação HTML e Razor** está em contraste a auxiliares HTML, a abordagem anterior para a criação do servidor de marcação nos modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-119">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="a6b1e-120">[Em comparação comparados auxiliares HTML de auxiliares de marcação](#tag-helpers-compared-to-html-helpers) explica as diferenças em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-120">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="a6b1e-121">[Suporte ao IntelliSense para os auxiliares de marcação](#intellisense-support-for-tag-helpers) explica o ambiente do IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-121">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="a6b1e-122">Até mesmo desenvolvedores experientes com sintaxe Razor c# são mais produtivos usando auxiliares de marcação que escrever marcação Razor c#.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-122">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="a6b1e-123">**Uma maneira de fazer a você mais produtivos e pode produzir mais robusta e confiável e um código usando as informações disponíveis apenas no servidor** , por exemplo, historicamente mantra imagens a atualização foi alterar o nome da imagem quando você alterar a imagem.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-123">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="a6b1e-124">Imagens devem ter cache agressivamente por motivos de desempenho e, em seguida, a menos que você altere o nome de uma imagem, você corre o risco de clientes obtendo uma cópia obsoleta.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-124">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="a6b1e-125">Historicamente, depois que uma imagem foi editada, o nome foram alterados e cada referência à imagem no aplicativo web precisa ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-125">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="a6b1e-126">Não só é muito trabalho intensivas, também é propensa (você pode perder uma referência, acidentalmente, insira a cadeia de caracteres incorreta, etc.) O interno `ImageTagHelper` pode fazer isso para você automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-126">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="a6b1e-127">O `ImageTagHelper` pode acrescentar uma versão de número para o nome da imagem, portanto, sempre que a imagem é alterada, o servidor gera automaticamente uma nova versão exclusiva para a imagem.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-127">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="a6b1e-128">Os clientes têm a garantia para obter a imagem atual.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-128">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="a6b1e-129">Esse aumento robustez e trabalho vem essencialmente livre usando o `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-129">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="a6b1e-130">A maioria os auxiliares de marca internos elementos HTML existentes de destino e fornece os atributos do lado do servidor para o elemento.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-130">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="a6b1e-131">Por exemplo, o `<input>` elemento usado em muitas das exibições no *modos de exibição/conta* pasta contém a `asp-for` atributo, que extrai o nome da propriedade do modelo especificado no HTML renderizado.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-131">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="a6b1e-132">A seguinte marcação Razor:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-132">The following Razor markup:</span></span>

```html
<label asp-for="Email"></label>
```

<span data-ttu-id="a6b1e-133">Gera o HTML a seguir:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-133">Generates the following HTML:</span></span>

```html
<label for="Email">Email</label>
```

<span data-ttu-id="a6b1e-134">O `asp-for` atributo é disponibilizado pelo `For` propriedade o `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-134">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="a6b1e-135">Consulte [criação auxiliares de marcação](authoring.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-135">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="a6b1e-136">Gerenciamento de escopo do auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="a6b1e-136">Managing Tag Helper scope</span></span>

<span data-ttu-id="a6b1e-137">Escopo de auxiliares de marca é controlado por uma combinação de `@addTagHelper`, `@removeTagHelper`e "!" recusar caractere.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-137">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="a6b1e-138">`@addTagHelper`disponibiliza auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="a6b1e-138">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="a6b1e-139">Se você criar um novo aplicativo de web do ASP.NET Core denominado *AuthoringTagHelpers* (com nenhuma autenticação), o seguinte *Views/_ViewImports.cshtml* arquivo será adicionado ao seu projeto:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-139">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="a6b1e-140">O `@addTagHelper` diretiva disponibiliza auxiliares de marcação para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-140">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="a6b1e-141">Nesse caso, o arquivo de exibição é *Views/_ViewImports.cshtml*, que por padrão é herdada por todos os arquivos de exibição no *exibições* pasta e seus subdiretórios; disponibilizar auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-141">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="a6b1e-142">O código anterior usa a sintaxe de curinga ("\*") para especificar que todos os auxiliares de marcação no assembly especificado (*Microsoft.AspNetCore.Mvc.TagHelpers*) estarão disponíveis para todos os arquivos no modo de exibição de *exibições* diretório ou subdiretório.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-142">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="a6b1e-143">O primeiro parâmetro após `@addTagHelper` Especifica os auxiliares de marca para carregar (estamos usando "\*" para todos os auxiliares de marcação), e o segundo parâmetro "Microsoft.AspNetCore.Mvc.TagHelpers" Especifica o assembly que contém os auxiliares de marca.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-143">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="a6b1e-144">*Microsoft.AspNetCore.Mvc.TagHelpers* é o assembly para os auxiliares de marca de núcleo ASP.NET interno.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-144">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="a6b1e-145">Para expor todos os auxiliares de marca neste projeto (que cria um assembly chamado *AuthoringTagHelpers*), você usaria o seguinte:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-145">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="a6b1e-146">Se o seu projeto contém um `EmailTagHelper` com o namespace padrão (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), você pode fornecer o nome totalmente qualificado (FQN) do auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-146">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="a6b1e-147">Para adicionar um auxiliar de marca para uma exibição usando um FQN, primeiro adicione o FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) e, em seguida, o nome do assembly (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-147">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="a6b1e-148">A maioria dos desenvolvedores preferem usar o "\*" sintaxe curingas.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-148">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="a6b1e-149">A sintaxe de caractere curinga permite que você insira o caractere curinga "\*" como o sufixo de um FQN.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-149">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="a6b1e-150">Por exemplo, qualquer um dos seguintes diretivas trará o `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-150">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="a6b1e-151">Conforme mencionado anteriormente, adicionando o `@addTagHelper` diretiva para o *Views/_ViewImports.cshtml* arquivo disponibiliza o auxiliar de marca para todos os arquivos de exibição no *exibições* diretório e subdiretórios.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-151">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="a6b1e-152">Você pode usar o `@addTagHelper` diretiva nos arquivos de modo de exibição específico se você deseja participar para expor o auxiliar de marca para apenas esses modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-152">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="a6b1e-153">`@removeTagHelper`Remove os auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="a6b1e-153">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="a6b1e-154">O `@removeTagHelper` tem os mesmos dois parâmetros `@addTagHelper`, e remove um auxiliar de marca foi adicionado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-154">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="a6b1e-155">Por exemplo, `@removeTagHelper` aplicado para remover um modo de exibição específico o auxiliar de marca especificado da exibição.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-155">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="a6b1e-156">Usando `@removeTagHelper` em uma *Views/Folder/_ViewImports.cshtml* arquivo remove o auxiliar de marca especificado de todas as exibições em *pasta*.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-156">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="a6b1e-157">Controlando o escopo do auxiliar de marca com o *viewimports. cshtml* arquivo</span><span class="sxs-lookup"><span data-stu-id="a6b1e-157">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="a6b1e-158">Você pode adicionar uma *viewimports. cshtml* para qualquer pasta de exibição e o modo de exibição mecanismo aplica as diretivas de ambas desse arquivo e o *Views/_ViewImports.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-158">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a6b1e-159">Se você adicionou um vazio *Views/Home/_ViewImports.cshtml* de arquivos para o *início* exibições, não haveria nenhuma alteração porque o *viewimports. cshtml* arquivo é aditivas.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-159">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="a6b1e-160">Qualquer `@addTagHelper` diretivas que você adicionar à *Views/Home/_ViewImports.cshtml* arquivo (que não estão no padrão *Views/_ViewImports.cshtml* arquivo) poderia expor os auxiliares de marcação para modos de exibição somente no *Início* pasta.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-160">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="a6b1e-161">Recusando elementos individuais</span><span class="sxs-lookup"><span data-stu-id="a6b1e-161">Opting out of individual elements</span></span>

<span data-ttu-id="a6b1e-162">Você pode desabilitar um auxiliar de marca no nível do elemento com o caractere de recusar auxiliar de marca ("!").</span><span class="sxs-lookup"><span data-stu-id="a6b1e-162">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="a6b1e-163">Por exemplo, `Email` validação está desabilitada no `<span>` com o caractere de recusar auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-163">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="a6b1e-164">Você deve aplicar o caractere de recusar auxiliar de marca para a marca de abertura e fechamento.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-164">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="a6b1e-165">(O editor do Visual Studio adiciona automaticamente o caractere de saída para a marca de fechamento quando você adiciona um para a marca de abertura).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-165">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="a6b1e-166">Depois de adicionar o caractere de recusa, o elemento e os atributos do auxiliar de marca não são exibidos em uma fonte diferente.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-166">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="a6b1e-167">Usando `@tagHelperPrefix` para fazer uso do auxiliar de marca explícita</span><span class="sxs-lookup"><span data-stu-id="a6b1e-167">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="a6b1e-168">O `@tagHelperPrefix` diretiva permite que você especifique uma cadeia de caracteres de prefixo de marca para habilitar o suporte auxiliar de marca e fazer uso do auxiliar de marca explícita.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-168">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="a6b1e-169">Por exemplo, você pode adicionar a seguinte marcação para o *Views/_ViewImports.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-169">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```html
@tagHelperPrefix th:
```
<span data-ttu-id="a6b1e-170">A imagem do código abaixo, o prefixo de marca auxiliar é definido como `th:`, portanto, somente esses elementos usando o prefixo `th:` suporte auxiliares de marcação (elementos de auxiliar de marca tem uma fonte diferente).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-170">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="a6b1e-171">O `<label>` e `<input>` elementos têm o prefixo de marca auxiliar e são habilitados para auxiliar de marca, enquanto o `<span>` elemento não.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-171">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element does not.</span></span>

![imagem](intro/_static/thp.png)

<span data-ttu-id="a6b1e-173">As mesmas regras de hierarquia que se aplicam a `@addTagHelper` também se aplicam a `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-173">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="a6b1e-174">Suporte ao IntelliSense para os auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="a6b1e-174">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="a6b1e-175">Quando você cria um novo aplicativo web ASP.NET no Visual Studio, ele adiciona o pacote NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="a6b1e-175">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="a6b1e-176">Este é o pacote que adiciona ferramentas de auxiliar de marca.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-176">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="a6b1e-177">Considere a possibilidade de gravar uma marca HTML `<label>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-177">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="a6b1e-178">Assim que você inserir `<l` no editor do Visual Studio, o IntelliSense exibe elementos correspondentes:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-178">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![imagem](intro/_static/label.png)

<span data-ttu-id="a6b1e-180">Não só você para obter ajuda em HTML, mas o ícone (o "@" símbolo com "<>" sob ele).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-180">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![imagem](intro/_static/tagSym.png)

<span data-ttu-id="a6b1e-182">identifica o elemento como alvo de auxiliares de marcação.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-182">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="a6b1e-183">Elementos HTML puros (como o `fieldset`) exibem o ícone de "<>".</span><span class="sxs-lookup"><span data-stu-id="a6b1e-183">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="a6b1e-184">Um HTML puro `<label>` marca exibe a marca HTML (com o tema de cores do Visual Studio padrão) em uma fonte marrom, os atributos em vermelho, e os valores de atributo em azul.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-184">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![imagem](intro/_static/LableHtmlTag.png)

<span data-ttu-id="a6b1e-186">Depois de inserir `<label`, IntelliSense listará os atributos HTML/CSS disponíveis e os atributos voltados para o auxiliar de marca:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-186">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![imagem](intro/_static/labelattr.png)

<span data-ttu-id="a6b1e-188">Preenchimento de instruções IntelliSense permite que você insira a tecla tab para completar a instrução com o valor selecionado:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-188">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![imagem](intro/_static/stmtcomplete.png)

<span data-ttu-id="a6b1e-190">Assim que um atributo do auxiliar de marca é inserido, alterar as fontes de marca e atributo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-190">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="a6b1e-191">Usando o padrão Visual Studio "Blue" ou tema de cores "Light", a fonte está em negrito roxo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-191">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="a6b1e-192">Se você estiver usando o tema "Escuro" a fonte está em negrito azul-petróleo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-192">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="a6b1e-193">As imagens neste documento foram feitas usando o tema padrão.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-193">The images in this document were taken using the default theme.</span></span>

![imagem](intro/_static/labelaspfor2.png)

<span data-ttu-id="a6b1e-195">Você pode inserir o Visual Studio *Palcompleta* atalho (Ctrl + Barra de espaço é o [padrão](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) dentro de aspas duplas (""), e você está agora no c#, exatamente como você deve estar em uma classe c#.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-195">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="a6b1e-196">O IntelliSense exibe todos os métodos e propriedades no modelo de página.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-196">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="a6b1e-197">Os métodos e propriedades estão disponíveis porque o tipo de propriedade é `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-197">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="a6b1e-198">Na imagem abaixo, eu estou editando o `Register` exibição, portanto, o `RegisterViewModel` está disponível.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-198">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![imagem](intro/_static/intellemail.png)

<span data-ttu-id="a6b1e-200">IntelliSense listará as propriedades e métodos disponíveis para o modelo na página.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-200">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="a6b1e-201">O ambiente de IntelliSense avançado ajuda a selecionar a classe CSS:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-201">The rich IntelliSense environment helps you select the CSS class:</span></span>

![imagem](intro/_static/iclass.png)

![imagem](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="a6b1e-204">Auxiliares de marcação em comparação comparados auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="a6b1e-204">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="a6b1e-205">Anexar auxiliares de marcação para elementos HTML em exibições Razor, enquanto [auxiliares HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) são invocados como métodos Intercalado com HTML nos modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-205">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="a6b1e-206">Considere a seguinte marcação Razor, que cria um rótulo HTML com a classe CSS "legenda":</span><span class="sxs-lookup"><span data-stu-id="a6b1e-206">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="a6b1e-207">O em (`@`) símbolo informa Razor, este é o início do código.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-207">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="a6b1e-208">Os dois parâmetros ("FirstName" e "nome:") são cadeias de caracteres, portanto [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) não pode ajudar.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-208">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="a6b1e-209">O último argumento:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-209">The last argument:</span></span>

```html
new {@class="caption"}
```

<span data-ttu-id="a6b1e-210">Um objeto anônimo é usado para representar os atributos.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-210">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="a6b1e-211">Porque **classe** é uma palavra reservada no c#, você deve usar o `@` símbolo forçar c# para interpretar "@class=" como um símbolo (nome da propriedade).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-211">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="a6b1e-212">Para um designer de front-end (alguém familiarizado com HTML/CSS/JavaScript e outras tecnologias de cliente, mas não está familiarizado com c# e Razor), a maior parte da linha é externa.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-212">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="a6b1e-213">Toda a linha deve ser criada com nenhuma ajuda do IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-213">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="a6b1e-214">Usando o `LabelTagHelper`, a mesma marcação pode ser escrita como:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-214">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![imagem](intro/_static/label2.png)

<span data-ttu-id="a6b1e-216">Com a versão do auxiliar de marca, assim que você inserir `<l` no editor do Visual Studio, o IntelliSense exibe elementos correspondentes:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-216">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![imagem](intro/_static/label.png)

<span data-ttu-id="a6b1e-218">IntelliSense ajuda você a escrever a linha inteira.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-218">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="a6b1e-219">O `LabelTagHelper` também padrão é definir o conteúdo do `asp-for` ("FirstName") do valor do atributo "Primeiro nome"; Ele converte propriedades concatenados em uma frase composta do nome da propriedade com um espaço onde ocorre a cada nova letra maiuscula.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-219">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="a6b1e-220">Na marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-220">In the following markup:</span></span>

![imagem](intro/_static/label2.png)

<span data-ttu-id="a6b1e-222">gera:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-222">generates:</span></span>

```html
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="a6b1e-223">O concatenados para conteúdo de maiusculas e minúsculas frase não será usado se você adicionar conteúdo para o `<label>`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-223">The camel-cased to sentence-cased content is not used if you add content to the `<label>`.</span></span> <span data-ttu-id="a6b1e-224">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-224">For example:</span></span>

![imagem](intro/_static/1stName.png)

<span data-ttu-id="a6b1e-226">gera:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-226">generates:</span></span>

```html
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="a6b1e-227">A imagem a seguir mostra a parte do formato da *Views/Account/Register.cshtml* exibição Razor gerada a partir de herdado ASP.NET 4.5 MVC modelo incluído com o Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-227">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![imagem](intro/_static/regCS.png)

<span data-ttu-id="a6b1e-229">Editor do Visual Studio exibe o código c# com um plano de fundo cinza.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-229">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="a6b1e-230">Por exemplo, o `AntiForgeryToken` auxiliar HTML:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-230">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```html
@Html.AntiForgeryToken()
```

<span data-ttu-id="a6b1e-231">é exibido com um plano de fundo cinza.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-231">is displayed with a grey background.</span></span> <span data-ttu-id="a6b1e-232">A maioria da marcação na exibição de registro é c#.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-232">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="a6b1e-233">Compare isso com a abordagem equivalente usando auxiliares de marca:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-233">Compare that to the equivalent approach using Tag Helpers:</span></span>

![imagem](intro/_static/regTH.png)

<span data-ttu-id="a6b1e-235">A marcação é muito limpas e fáceis de ler, editar e manter-se que a abordagem de auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-235">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="a6b1e-236">O código c# é reduzido ao mínimo que o servidor precisa conhecer.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-236">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="a6b1e-237">Editor do Visual Studio exibe marcação direcionada por um auxiliar de marca em uma fonte diferente.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-237">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="a6b1e-238">Considere o *Email* grupo:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-238">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="a6b1e-239">Cada um dos atributos "asp-" tem um valor de "Email", mas "Email" não é uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-239">Each of the "asp-" attributes has a value of "Email", but "Email" is not a string.</span></span> <span data-ttu-id="a6b1e-240">Nesse contexto, "Email" é a c# expressão propriedade do modelo para o `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-240">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="a6b1e-241">O editor do Visual Studio ajuda você a escrever **todas as** da marcação no método auxiliar de marca de formulário de registro, enquanto o Visual Studio não fornece nenhuma ajuda para a maioria do código na abordagem auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-241">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="a6b1e-242">[Suporte ao IntelliSense para os auxiliares de marcação](#intellisense-support-for-tag-helpers) apresenta detalhes sobre como trabalhar com auxiliares de marcação no editor do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-242">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="a6b1e-243">Auxiliares de marcação em comparação comparados controles de servidor Web</span><span class="sxs-lookup"><span data-stu-id="a6b1e-243">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="a6b1e-244">Auxiliares de marcação não tem o elemento que estiverem associados; simplesmente participam no processamento do elemento e conteúdo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-244">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="a6b1e-245">ASP.NET [controles de servidor Web](https://msdn.microsoft.com/library/7698y1f0.aspx) são declarados e invocado em uma página.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-245">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="a6b1e-246">[Controles de servidor Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) têm um ciclo de vida não trivial que pode dificultar o desenvolvimento e depuração.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-246">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="a6b1e-247">Controles de servidor Web permitem que você adicionar a funcionalidade para os elementos de modelo de objeto de documento (DOM) do cliente usando um controle de cliente.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-247">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="a6b1e-248">Auxiliares de marcação não tem nenhum DOM.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-248">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="a6b1e-249">Controles de servidor Web incluem a detecção automática do navegador.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-249">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="a6b1e-250">Auxiliares de marcação não têm nenhum conhecimento do navegador.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-250">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="a6b1e-251">Vários auxiliares de marcação pode agir no mesmo elemento (consulte [conflitos evitando auxiliar de marca](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) enquanto você normalmente não é possível compor controles de servidor Web.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-251">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="a6b1e-252">Auxiliares de marcação podem modificar a marca e o conteúdo de elementos HTML que eles estiverem no escopo, mas não modifique diretamente tudo em uma página.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-252">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="a6b1e-253">Controles de servidor Web tem um escopo de menos específico e podem executar ações que afetam outras partes da página; Habilitando efeitos colaterais não intencionais.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-253">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="a6b1e-254">Controles de servidor Web usam conversores de tipo para converter cadeias de caracteres em objetos.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-254">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="a6b1e-255">Com auxiliares de marca, você trabalha nativamente em c#, portanto você não precisa de conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-255">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="a6b1e-256">Uso de controles de servidor Web [System. ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) para implementar o comportamento de tempo de execução e tempo de design de componentes e controles.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-256">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="a6b1e-257">`System.ComponentModel`inclui as classes e interfaces base para implementar atributos e conversores de tipo, associando a dados de fontes e licenciamento de componentes.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-257">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="a6b1e-258">Compare isso com a auxiliares de marca, que normalmente derivam `TagHelper`e o `TagHelper` classe base expõe apenas dois métodos, `Process` e `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="a6b1e-258">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="a6b1e-259">Personalizando a fonte de elemento do auxiliar de marca</span><span class="sxs-lookup"><span data-stu-id="a6b1e-259">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="a6b1e-260">Você pode personalizar a fonte e a coloração de **ferramentas** > **opções** > **ambiente** > **fontes Cores e**:</span><span class="sxs-lookup"><span data-stu-id="a6b1e-260">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![imagem](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="a6b1e-262">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="a6b1e-262">Additional Resources</span></span>

* [<span data-ttu-id="a6b1e-263">Criando auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="a6b1e-263">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="a6b1e-264">Trabalhando com formulários</span><span class="sxs-lookup"><span data-stu-id="a6b1e-264">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="a6b1e-265">[TagHelperSamples no GitHub](https://github.com/dpaquette/TagHelperSamples) contém exemplos de auxiliar de marca para trabalhar com [inicialização](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a6b1e-265">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
