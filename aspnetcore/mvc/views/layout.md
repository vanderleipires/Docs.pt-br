---
title: Layout
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 25aa5fc730d9076fdcf9d29cb5d9dfa75a246a1a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="layout"></a><span data-ttu-id="1b7bb-103">Layout</span><span class="sxs-lookup"><span data-stu-id="1b7bb-103">Layout</span></span>

<span data-ttu-id="1b7bb-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1b7bb-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1b7bb-105">Modos de exibição frequentemente compartilham elementos visuais e através de programação.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="1b7bb-106">Neste artigo, você aprenderá a usar layouts comuns, compartilhar diretivas e executar o código comum antes de modos de exibição de renderização em seu aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="1b7bb-107">O que é um Layout</span><span class="sxs-lookup"><span data-stu-id="1b7bb-107">What is a Layout</span></span>

<span data-ttu-id="1b7bb-108">A maioria dos aplicativos web têm um layout comum que fornece aos usuários uma experiência consistente conforme eles navegam de uma página.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="1b7bb-109">O layout normalmente inclui elementos de interface do usuário comum, como o cabeçalho de aplicativo, navegação ou elementos de menu e rodapé.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Exemplo de Layout de página](layout/_static/page-layout.png)

<span data-ttu-id="1b7bb-111">Estruturas HTML comuns, como scripts e folhas de estilo são também usadas pelo número de páginas dentro de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="1b7bb-112">Todos esses elementos compartilhados podem ser definidos em uma *layout* arquivo, que pode ser referenciado por qualquer exibição usada dentro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="1b7bb-113">Layouts de reduzem código duplicado em exibições, ajudando a seguir o [princípio não repita por conta própria (teste)](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="1b7bb-114">Por convenção, o layout padrão para um aplicativo ASP.NET é denominado `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="1b7bb-115">O modelo de projeto do Visual Studio ASP.NET Core MVC inclui o arquivo de layout no `Views/Shared` pasta:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![pasta de modos de exibição no Gerenciador de soluções](layout/_static/web-project-views.png)

<span data-ttu-id="1b7bb-117">Este layout define um modelo de nível superior para modos de exibição no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="1b7bb-118">Aplicativos não requerem um layout e aplicativos podem definir mais de um layout, com diferentes modos de exibição especificando layouts diferentes.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="1b7bb-119">Um exemplo `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-119">An example `_Layout.cshtml`:</span></span>

<span data-ttu-id="1b7bb-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span><span class="sxs-lookup"><span data-stu-id="1b7bb-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span></span>

## <a name="specifying-a-layout"></a><span data-ttu-id="1b7bb-121">Especificando um Layout</span><span class="sxs-lookup"><span data-stu-id="1b7bb-121">Specifying a Layout</span></span>

<span data-ttu-id="1b7bb-122">Modos de exibição Razor têm um `Layout` propriedade.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-122">Razor views have a `Layout` property.</span></span> <span data-ttu-id="1b7bb-123">Modos de exibição individuais Especifica um layout definindo essa propriedade:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-123">Individual views specify a layout by setting this property:</span></span>

<span data-ttu-id="1b7bb-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="1b7bb-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span></span>

<span data-ttu-id="1b7bb-125">O layout especificado pode usar um caminho completo (exemplo: `/Views/Shared/_Layout.cshtml`) ou um nome parcial (exemplo: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-125">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="1b7bb-126">Quando é fornecido um nome parcial, o mecanismo de exibição Razor procurará o arquivo de layout usando seu processo de descoberta padrão.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-126">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="1b7bb-127">A pasta associada de controlador é pesquisada primeiro, seguido de `Shared` pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-127">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="1b7bb-128">Esse processo de descoberta é idêntico àquele usado para descobrir [exibições parciais](partial.md).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-128">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="1b7bb-129">Por padrão, todo layout deve chamar `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-129">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="1b7bb-130">Sempre que a chamada para `RenderBody` é colocada, o conteúdo do modo de exibição será renderizado.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-130">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name=layout-sections-label></a>

### <a name="sections"></a><span data-ttu-id="1b7bb-131">Seções</span><span class="sxs-lookup"><span data-stu-id="1b7bb-131">Sections</span></span>

<span data-ttu-id="1b7bb-132">Um layout, opcionalmente, pode fazer referência a um ou mais *seções*, chamando `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-132">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="1b7bb-133">Seções fornecem uma maneira de organizar onde determinados elementos da página devem ser colocados.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-133">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="1b7bb-134">Cada chamada para `RenderSection` pode especificar se essa seção é obrigatória ou opcional.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-134">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="1b7bb-135">Se uma seção obrigatória não for encontrada, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-135">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="1b7bb-136">Modos de exibição individuais especificam o conteúdo a ser renderizados dentro de uma seção usando o `@section` a sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-136">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="1b7bb-137">Se uma exibição define uma seção, deve ser processado ou ocorrerá um erro.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-137">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="1b7bb-138">Um exemplo `@section` definição em um modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-138">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="1b7bb-139">No código acima, os scripts de validação são adicionados para o `scripts` seção em uma exibição que inclui um formulário.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-139">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="1b7bb-140">Outros modos de exibição no mesmo aplicativo podem não exigir qualquer script adicional e portanto não precisam definir uma seção de scripts.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-140">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="1b7bb-141">Seções definidas em um modo de exibição estão disponíveis apenas em sua página de layout imediata.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-141">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="1b7bb-142">Eles não podem ser referenciados de existe meio-termo, componentes do modo de exibição ou outras partes do sistema de exibição.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-142">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="1b7bb-143">Ignorando seções</span><span class="sxs-lookup"><span data-stu-id="1b7bb-143">Ignoring sections</span></span>

<span data-ttu-id="1b7bb-144">Por padrão, o corpo e todas as seções de uma página de conteúdo devem todas ser renderizadas por página de layout.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-144">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="1b7bb-145">O mecanismo de exibição Razor impõe esse controle se o corpo e cada seção foi renderizados.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-145">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="1b7bb-146">Para instruir o mecanismo de exibição para ignorar o corpo ou seções, chame o `IgnoreBody` e `IgnoreSection` métodos.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-146">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="1b7bb-147">O corpo e cada seção em uma página de Razor devem ser processados ou ignorados.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-147">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name=viewimports></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="1b7bb-148">Importar diretivas compartilhadas</span><span class="sxs-lookup"><span data-stu-id="1b7bb-148">Importing Shared Directives</span></span>

<span data-ttu-id="1b7bb-149">Modos de exibição podem usar diretivas do Razor para muitas coisas, como importar namespaces ou executar [injeção de dependência](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-149">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="1b7bb-150">Diretivas compartilhadas por vários modos de exibição podem ser especificadas em um comum `_ViewImports.cshtml` arquivo.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-150">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="1b7bb-151">O `_ViewImports` arquivo suporta as seguintes diretivas:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-151">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="1b7bb-152">O arquivo não oferece suporte a outros recursos do Razor, como funções e definições de seção.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-152">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="1b7bb-153">Um exemplo de `_ViewImports.cshtml` arquivo:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-153">A sample `_ViewImports.cshtml` file:</span></span>

<span data-ttu-id="1b7bb-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="1b7bb-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="1b7bb-155">O `_ViewImports.cshtml` de arquivos para um aplicativo MVC do ASP.NET Core normalmente é colocado no `Views` pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-155">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="1b7bb-156">Um `_ViewImports.cshtml` arquivo pode ser colocado em qualquer pasta na qual o caso só serão aplicada a exibições dentro dessa pasta e suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-156">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="1b7bb-157">`_ViewImports`os arquivos são processados, iniciando no nível raiz, e, em seguida, para cada pasta à esquerda até o local da exibição em si, para que as configurações especificadas no nível raiz pode ser substituído no nível da pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-157">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="1b7bb-158">Por exemplo, se um nível de raiz `_ViewImports.cshtml` arquivo especifica `@model` e `@addTagHelper`e outro `_ViewImports.cshtml` arquivo na pasta do modo de exibição associado a controlador Especifica outro `@model` e adiciona outro `@addTagHelper`, o modo de exibição terão acesso a ambos os auxiliares de marcação e usará o último `@model`.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-158">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="1b7bb-159">Se vários `_ViewImports.cshtml` arquivos sejam executados por um modo de exibição, combinados comportamento das diretivas incluídas no `ViewImports.cshtml` arquivos será da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-159">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="1b7bb-160">`@addTagHelper`, `@removeTagHelper`: execução, em ordem</span><span class="sxs-lookup"><span data-stu-id="1b7bb-160">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="1b7bb-161">`@tagHelperPrefix`: o mais próximo um para o modo de exibição substitui quaisquer outras</span><span class="sxs-lookup"><span data-stu-id="1b7bb-161">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1b7bb-162">`@model`: o mais próximo um para o modo de exibição substitui quaisquer outras</span><span class="sxs-lookup"><span data-stu-id="1b7bb-162">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1b7bb-163">`@inherits`: o mais próximo um para o modo de exibição substitui quaisquer outras</span><span class="sxs-lookup"><span data-stu-id="1b7bb-163">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="1b7bb-164">`@using`: todos são incluídos. duplicatas serão ignoradas</span><span class="sxs-lookup"><span data-stu-id="1b7bb-164">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="1b7bb-165">`@inject`: para cada propriedade, aquele mais próximo à exibição substitui quaisquer outros usuários com o mesmo nome de propriedade</span><span class="sxs-lookup"><span data-stu-id="1b7bb-165">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name=viewstart></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="1b7bb-166">Executar código antes de cada modo de exibição</span><span class="sxs-lookup"><span data-stu-id="1b7bb-166">Running Code Before Each View</span></span>

<span data-ttu-id="1b7bb-167">Se você tiver o código necessário executar antes de cada exibição, isso deve ser colocado no `_ViewStart.cshtml` arquivo.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-167">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="1b7bb-168">Por convenção, o `_ViewStart.cshtml` arquivo está localizado no `Views` pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-168">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="1b7bb-169">As instruções listadas no `_ViewStart.cshtml` são executados antes de cada exibição completa (não layouts e exibições parciais não).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-169">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="1b7bb-170">Como [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` são hierárquicos.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-170">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="1b7bb-171">Se um `_ViewStart.cshtml` arquivo é definido na pasta de exibição associada por controlador, ele será executado depois de definido na raiz do `Views` pasta (se houver).</span><span class="sxs-lookup"><span data-stu-id="1b7bb-171">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="1b7bb-172">Um exemplo de `_ViewStart.cshtml` arquivo:</span><span class="sxs-lookup"><span data-stu-id="1b7bb-172">A sample `_ViewStart.cshtml` file:</span></span>

<span data-ttu-id="1b7bb-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="1b7bb-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="1b7bb-174">O arquivo acima Especifica que todas as exibições usarão o `_Layout.cshtml` layout.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-174">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="1b7bb-175">Nem `_ViewStart.cshtml` nem `_ViewImports.cshtml` geralmente são colocadas as `/Views/Shared` pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-175">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="1b7bb-176">As versões de nível de aplicativo desses arquivos devem ser colocadas diretamente no `/Views` pasta.</span><span class="sxs-lookup"><span data-stu-id="1b7bb-176">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
