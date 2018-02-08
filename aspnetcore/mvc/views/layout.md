---
title: Layout
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 3e9e5949d8940a33508e24f0da015b49b7ba468c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="layout"></a><span data-ttu-id="eb9a0-102">Layout</span><span class="sxs-lookup"><span data-stu-id="eb9a0-102">Layout</span></span>

<span data-ttu-id="eb9a0-103">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="eb9a0-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="eb9a0-104">Com frequência, as exibições compartilham elementos visuais e programáticos.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-104">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="eb9a0-105">Neste artigo, você aprenderá a usar layouts comuns, compartilhar diretivas e executar o código comum antes de renderizar exibições no aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-105">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="eb9a0-106">O que é um layout</span><span class="sxs-lookup"><span data-stu-id="eb9a0-106">What is a Layout</span></span>

<span data-ttu-id="eb9a0-107">A maioria dos aplicativos Web tem um layout comum que fornece aos usuários uma experiência consistente durante sua navegação de uma página a outra.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-107">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="eb9a0-108">O layout normalmente inclui elementos comuns de interface do usuário, como o cabeçalho do aplicativo, elementos de menu ou de navegação e rodapé.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-108">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Exemplo de layout da página](layout/_static/page-layout.png)

<span data-ttu-id="eb9a0-110">Estruturas HTML comuns, como scripts e folhas de estilo, também são usadas com frequência por muitas páginas em um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-110">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="eb9a0-111">Todos esses elementos compartilhados podem ser definidos em um arquivo de *layout*, que pode então ser referenciado por qualquer exibição usada no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-111">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="eb9a0-112">Os layouts reduzem o código duplicado em exibições, ajudando-os a seguir o [princípio DRY (Don't Repeat Yourself)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-112">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="eb9a0-113">Por convenção, o layout padrão de um aplicativo ASP.NET é chamado `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-113">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="eb9a0-114">O modelo de projeto do ASP.NET Core MVC no Visual Studio inclui o arquivo de layout na pasta `Views/Shared`:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-114">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![pasta de exibições no gerenciador de soluções](layout/_static/web-project-views.png)

<span data-ttu-id="eb9a0-116">Esse layout define um modelo de nível superior para exibições no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-116">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="eb9a0-117">Os aplicativos não exigem um layout e podem definir mais de um layout, com diferentes exibições especificando diferentes layouts.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-117">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="eb9a0-118">Um `_Layout.cshtml` de exemplo:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-118">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="eb9a0-119">Especificando um layout</span><span class="sxs-lookup"><span data-stu-id="eb9a0-119">Specifying a Layout</span></span>

<span data-ttu-id="eb9a0-120">As exibições do Razor têm uma propriedade `Layout`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-120">Razor views have a `Layout` property.</span></span> <span data-ttu-id="eb9a0-121">As exibições individuais especificam um layout com a configuração dessa propriedade:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-121">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="eb9a0-122">O layout especificado pode usar um caminho completo (exemplo: `/Views/Shared/_Layout.cshtml`) ou um nome parcial (exemplo: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-122">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="eb9a0-123">Quando um nome parcial for fornecido, o mecanismo de exibição do Razor pesquisará o arquivo de layout usando seu processo de descoberta padrão.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-123">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="eb9a0-124">A pasta associada ao controlador é pesquisada primeiro, seguida da pasta `Shared`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-124">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="eb9a0-125">Esse processo de descoberta é idêntico àquele usado para descobrir [exibições parciais](partial.md).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-125">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="eb9a0-126">Por padrão, todo layout precisa chamar `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-126">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="eb9a0-127">Sempre que a chamada a `RenderBody` for feita, o conteúdo da exibição será renderizado.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-127">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="eb9a0-128">Seções</span><span class="sxs-lookup"><span data-stu-id="eb9a0-128">Sections</span></span>

<span data-ttu-id="eb9a0-129">Um layout, opcionalmente, pode referenciar uma ou mais *seções*, chamando `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-129">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="eb9a0-130">As seções fornecem uma maneira de organizar o local em que determinados elementos da página devem ser colocados.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-130">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="eb9a0-131">Cada chamada a `RenderSection` pode especificar se essa seção é obrigatória ou opcional.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-131">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="eb9a0-132">Se uma seção obrigatória não for encontrada, uma exceção será gerada.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-132">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="eb9a0-133">As exibições individuais especificam o conteúdo a ser renderizado em uma seção usando a sintaxe Razor `@section`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-133">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="eb9a0-134">Se uma exibição definir uma seção, ela precisará ser renderizada (ou ocorrerá um erro).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-134">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="eb9a0-135">Uma definição `@section` de exemplo em uma exibição:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-135">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="eb9a0-136">No código acima, os scripts de validação são adicionados à seção `scripts` em uma exibição que inclui um formulário.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-136">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="eb9a0-137">Outras exibições no mesmo aplicativo podem não exigir scripts adicionais e, portanto, não precisam definir uma seção de scripts.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-137">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="eb9a0-138">As seções definidas em uma exibição estão disponíveis apenas em sua página de layout imediata.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-138">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="eb9a0-139">Elas não podem ser referenciadas em parciais, componentes de exibição ou outras partes do sistema de exibição.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-139">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="eb9a0-140">Ignorando seções</span><span class="sxs-lookup"><span data-stu-id="eb9a0-140">Ignoring sections</span></span>

<span data-ttu-id="eb9a0-141">Por padrão, o corpo e todas as seções de uma página de conteúdo precisam ser renderizados pela página de layout.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-141">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="eb9a0-142">O mecanismo de exibição do Razor impõe isso acompanhando se o corpo e cada seção foram renderizados.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-142">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="eb9a0-143">Para instruir o mecanismo de exibição a ignorar o corpo ou as seções, chame os métodos `IgnoreBody` e `IgnoreSection`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-143">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="eb9a0-144">O corpo e cada seção em uma página do Razor precisam ser renderizados ou ignorados.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-144">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="eb9a0-145">Importando diretivas compartilhadas</span><span class="sxs-lookup"><span data-stu-id="eb9a0-145">Importing Shared Directives</span></span>

<span data-ttu-id="eb9a0-146">As exibições podem usar diretivas do Razor para fazer muitas coisas, como importar namespaces ou executar a [injeção de dependência](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-146">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="eb9a0-147">Diretivas compartilhadas por muitas exibições podem ser especificadas em um arquivo `_ViewImports.cshtml` comum.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-147">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="eb9a0-148">O arquivo `_ViewImports` dá suporte às seguintes diretivas:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-148">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="eb9a0-149">O arquivo não dá suporte a outros recursos do Razor, como funções e definições de seção.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-149">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="eb9a0-150">Um arquivo `_ViewImports.cshtml` de exemplo:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-150">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="eb9a0-151">O arquivo `_ViewImports.cshtml` para um aplicativo ASP.NET Core MVC normalmente é colocado na pasta `Views`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-151">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="eb9a0-152">Um arquivo `_ViewImports.cshtml` pode ser colocado em qualquer pasta, caso em que só será aplicado a exibições nessa pasta e em suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-152">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="eb9a0-153">Os arquivos `_ViewImports` são processados, começando no nível raiz e, em seguida, para cada pasta até o local da exibição em si. Portanto, as configurações especificadas no nível raiz podem ser substituídas no nível da pasta.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-153">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="eb9a0-154">Por exemplo, se um arquivo `_ViewImports.cshtml` no nível raiz especificar `@model` e `@addTagHelper` e outro arquivo `_ViewImports.cshtml` na pasta associada ao controlador da exibição especificar um `@model` diferente e adicionar outro `@addTagHelper`, a exibição terá acesso aos dois auxiliares de marca e usará o último `@model`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-154">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="eb9a0-155">Se vários arquivos `_ViewImports.cshtml` forem executados para uma exibição, o comportamento combinado das diretivas incluídas nos arquivos `ViewImports.cshtml` será o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-155">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="eb9a0-156">`@addTagHelper`, `@removeTagHelper`: todos são executados, em ordem</span><span class="sxs-lookup"><span data-stu-id="eb9a0-156">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="eb9a0-157">`@tagHelperPrefix`: o mais próximo à exibição substitui todos os outros</span><span class="sxs-lookup"><span data-stu-id="eb9a0-157">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="eb9a0-158">`@model`: o mais próximo à exibição substitui todos os outros</span><span class="sxs-lookup"><span data-stu-id="eb9a0-158">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="eb9a0-159">`@inherits`: o mais próximo à exibição substitui todos os outros</span><span class="sxs-lookup"><span data-stu-id="eb9a0-159">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="eb9a0-160">`@using`: todos são incluídos; duplicatas são ignoradas</span><span class="sxs-lookup"><span data-stu-id="eb9a0-160">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="eb9a0-161">`@inject`: para cada propriedade, o mais próximo à exibição substitui todos os outros com o mesmo nome de propriedade</span><span class="sxs-lookup"><span data-stu-id="eb9a0-161">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="eb9a0-162">Executando o código antes de cada exibição</span><span class="sxs-lookup"><span data-stu-id="eb9a0-162">Running Code Before Each View</span></span>

<span data-ttu-id="eb9a0-163">Se você tiver o código necessário a ser executado antes de cada exibição, isso deverá ser colocado no arquivo `_ViewStart.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-163">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="eb9a0-164">Por convenção, o arquivo `_ViewStart.cshtml` está localizado na pasta `Views`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-164">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="eb9a0-165">As instruções listadas em `_ViewStart.cshtml` são executadas antes de cada exibição completa (não layouts nem exibições parciais).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-165">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="eb9a0-166">Como [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` é hierárquico.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-166">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="eb9a0-167">Se um arquivo `_ViewStart.cshtml` for definido na pasta de exibição associada ao controlador, ele será executado depois daquele definido na raiz da pasta `Views` (se houver).</span><span class="sxs-lookup"><span data-stu-id="eb9a0-167">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="eb9a0-168">Um arquivo `_ViewStart.cshtml` de exemplo:</span><span class="sxs-lookup"><span data-stu-id="eb9a0-168">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="eb9a0-169">O arquivo acima especifica que todas as exibições usarão o layout `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-169">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="eb9a0-170">`_ViewStart.cshtml` nem `_ViewImports.cshtml` geralmente são colocados na pasta `/Views/Shared`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-170">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="eb9a0-171">As versões no nível do aplicativo desses arquivos devem ser colocadas diretamente na pasta `/Views`.</span><span class="sxs-lookup"><span data-stu-id="eb9a0-171">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
