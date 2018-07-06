---
uid: mvc/overview/releases/mvc51-release-notes
title: O que há de novo no ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825642"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="6a65c-102">O que há de novo no ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="6a65c-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="6a65c-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6a65c-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="6a65c-104">Este tópico descreve o que há de novo para ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="6a65c-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="6a65c-105">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="6a65c-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="6a65c-106">Baixar</span><span class="sxs-lookup"><span data-stu-id="6a65c-106">Download</span></span>](#download)
- [<span data-ttu-id="6a65c-107">Documentação</span><span class="sxs-lookup"><span data-stu-id="6a65c-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="6a65c-108">Novos recursos no ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="6a65c-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="6a65c-109">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="6a65c-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="6a65c-110">Suporte de inicialização para modelos de editor</span><span class="sxs-lookup"><span data-stu-id="6a65c-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="6a65c-111">Suporte a enum nos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="6a65c-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="6a65c-112">Validação não invasiva para MinLength/MaxLength atributos</span><span class="sxs-lookup"><span data-stu-id="6a65c-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="6a65c-113">Suporte a contexto 'this' em Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="6a65c-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="6a65c-114">[Problemas conhecidos e alterações significativas](#KnownBreakingChanges)- [correções de bugs](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="6a65c-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="6a65c-115">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="6a65c-115">Software Requirements</span></span>

- <span data-ttu-id="6a65c-116">Visual Studio 2012: Baixe o [ASP.NET e Web Tools 2013.1 para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="6a65c-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="6a65c-117">Visual Studio 2013: Baixe o [Visual Studio 2013 atualização 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="6a65c-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="6a65c-118">Esta atualização é necessária para a edição de exibições do Razor do ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="6a65c-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="6a65c-119">Baixar</span><span class="sxs-lookup"><span data-stu-id="6a65c-119">Download</span></span>

<span data-ttu-id="6a65c-120">Os recursos de tempo de execução são lançados como pacotes NuGet na Galeria do NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a65c-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="6a65c-121">Todos os pacotes de tempo de execução execute as [controle de versão semântico](http://semver.org/) especificação.</span><span class="sxs-lookup"><span data-stu-id="6a65c-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="6a65c-122">O pacote mais recente da versão RTM do ASP.NET MVC 5.1 tem a seguinte versão: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="6a65c-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="6a65c-123">Você pode instalar ou atualizar esses pacotes por meio [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="6a65c-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="6a65c-124">A versão também inclui pacotes localizados correspondentes no NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a65c-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="6a65c-125">Você pode instalar ou atualizar para os pacotes do NuGet lançados por meio do Console do Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="6a65c-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="6a65c-126">Documentação</span><span class="sxs-lookup"><span data-stu-id="6a65c-126">Documentation</span></span>

<span data-ttu-id="6a65c-127">Tutoriais e outras informações sobre a versão RTM do ASP.NET MVC 5.1 estão disponíveis no site da web do ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="6a65c-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="6a65c-128">Novos recursos no ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="6a65c-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="6a65c-129">Aprimoramentos de roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="6a65c-129">Attribute routing improvements</span></span>

 <span data-ttu-id="6a65c-130">Roteamento agora dá suporte a restrições, habilitando o controle de versão e o cabeçalho de atributo com base em seleção de rotas.</span><span class="sxs-lookup"><span data-stu-id="6a65c-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="6a65c-131">Muitos aspectos de rotas de atributo agora podem ser personalizados por meio de `IDirectRouteFactory` interface e `RouteFactoryAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="6a65c-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="6a65c-132">O prefixo da rota agora é extensível por meio de `IRoutePrefix` interface e `RoutePrefixAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="6a65c-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="6a65c-133">Suporte a enum nos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="6a65c-133">Enum support in views</span></span>

1. <span data-ttu-id="6a65c-134">Novo `@Html.EnumDropDownListFor()` métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="6a65c-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="6a65c-135">Eles devem ser usados como a maioria dos auxiliares HTML com a ressalva de que a expressão deve ser avaliada como um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo ou um [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) onde `T` é um [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo.</span><span class="sxs-lookup"><span data-stu-id="6a65c-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="6a65c-136">Use `EnumHelper.IsValidForEnumHelper()` estes requisitos.</span><span class="sxs-lookup"><span data-stu-id="6a65c-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="6a65c-137">Novos `EnumHelper.GetSelectList()` métodos que retornam um `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="6a65c-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="6a65c-138">Isso é útil quando você precisar manipular uma lista de seleção antes de chamar, por exemplo, `@Html.DropDownListFor()`, ou quando você deseja exibir os nomes que `@Html.EnumDropDownListFor()` mostra.</span><span class="sxs-lookup"><span data-stu-id="6a65c-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="6a65c-139">O código a seguir mostra essas APIs.</span><span class="sxs-lookup"><span data-stu-id="6a65c-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="6a65c-140">Você pode ver um exemplo completo [aqui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="6a65c-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="6a65c-141">Suporte de inicialização para modelos de editor</span><span class="sxs-lookup"><span data-stu-id="6a65c-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="6a65c-142">Agora podemos permitir a passagem de atributos HTML na [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) como um [objeto anônimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a65c-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="6a65c-143">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6a65c-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="6a65c-144">Validação não invasiva para MinLengthAttribute e MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="6a65c-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="6a65c-145">Validação do lado do cliente para tipos de cadeia de caracteres e matriz agora terá suporte para as propriedades decoradas com o [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atributos.</span><span class="sxs-lookup"><span data-stu-id="6a65c-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="6a65c-146">Suporte a contexto 'this' em Ajax discreto</span><span class="sxs-lookup"><span data-stu-id="6a65c-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="6a65c-147">As funções de retorno de chamada (`OnBegin, OnComplete, OnFailure, OnSuccess`) agora será capaz de localizar o elemento invocando via o `this` contexto.</span><span class="sxs-lookup"><span data-stu-id="6a65c-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="6a65c-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="6a65c-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="6a65c-149">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="6a65c-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="6a65c-150">Roteamento de atributo</span><span class="sxs-lookup"><span data-stu-id="6a65c-150">Attribute Routing</span></span>

<span data-ttu-id="6a65c-151">Ambiguidades em correspondências de roteamento de atributo agora relatará um erro em vez de escolher a primeira correspondência.</span><span class="sxs-lookup"><span data-stu-id="6a65c-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="6a65c-152">Rotas de atributo estão proibidas de usar o `{controller}` parâmetro e que usa o `{action}` colocado de parâmetro nas rotas em ações.</span><span class="sxs-lookup"><span data-stu-id="6a65c-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="6a65c-153">Usa um desses parâmetros muito provavelmente poderia levar a ambiguidades.</span><span class="sxs-lookup"><span data-stu-id="6a65c-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="6a65c-154">O scaffolding de API da Web do MVC em um projeto com resultados de pacotes 5.1 em 5.0 pacotes para aqueles que já não existem no projeto</span><span class="sxs-lookup"><span data-stu-id="6a65c-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="6a65c-155">Atualizando pacotes do NuGet para o RTM do ASP.NET MVC 5.1 não atualiza as ferramentas do Visual Studio, como o scaffolding do ASP.NET ou o modelo de projeto de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6a65c-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="6a65c-156">Eles usam a versão anterior dos pacotes de tempo de execução do ASP.NET (Version=5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="6a65c-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="6a65c-157">Como resultado, o scaffolding do ASP.NET instalará a versão anterior (Version=5.0.0.0) dos pacotes necessários, se eles já não estão disponíveis em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="6a65c-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="6a65c-158">No entanto, o scaffolding do ASP.NET no Visual Studio 2013 RTM ou atualização 1 não substituirá os pacotes mais recentes em seus projetos.</span><span class="sxs-lookup"><span data-stu-id="6a65c-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="6a65c-159">Se você usar o scaffolding do ASP.NET depois de atualizar os pacotes de seus projetos Web API 2.1 ou ASP.NET MVC 5.1, verifique se que as versões de API da Web e ASP.NET MVC são consistentes.</span><span class="sxs-lookup"><span data-stu-id="6a65c-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="6a65c-160">Realce de sintaxe para as exibições do Razor no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6a65c-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="6a65c-161">Se você atualizar para o RTM do ASP.NET MVC 5.1 sem atualizar o Visual Studio 2013, você não terá suporte ao editor do Visual Studio para o realce de sintaxe ao editar os modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="6a65c-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="6a65c-162">Você precisará atualizar o Visual Studio 2013 para obter esse suporte.</span><span class="sxs-lookup"><span data-stu-id="6a65c-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="6a65c-163">Renomeações de tipo</span><span class="sxs-lookup"><span data-stu-id="6a65c-163">Type Renames</span></span>

<span data-ttu-id="6a65c-164">Alguns dos tipos usados para extensibilidade de roteamento de atributo são renomeados no RTM 5.1.</span><span class="sxs-lookup"><span data-stu-id="6a65c-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="6a65c-165">**Nome do tipo antigo (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="6a65c-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="6a65c-166">**Novo tipo de nome (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="6a65c-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="6a65c-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="6a65c-167">IDirectRouteProvider</span></span> | <span data-ttu-id="6a65c-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="6a65c-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="6a65c-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="6a65c-169">RouteProviderAttribute</span></span> | <span data-ttu-id="6a65c-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="6a65c-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="6a65c-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="6a65c-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="6a65c-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="6a65c-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="6a65c-173">Correções de Bug</span><span class="sxs-lookup"><span data-stu-id="6a65c-173">Bug Fixes</span></span>

<span data-ttu-id="6a65c-174">Esta versão também inclui várias correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="6a65c-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="6a65c-175">Você pode encontrar a lista completa aqui:</span><span class="sxs-lookup"><span data-stu-id="6a65c-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="6a65c-176">5.1.0 pacote</span><span class="sxs-lookup"><span data-stu-id="6a65c-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="6a65c-177">5.1.1 pacote de</span><span class="sxs-lookup"><span data-stu-id="6a65c-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="6a65c-178">O 5.1.2 pacote contém as atualizações do IntelliSense, mas não há correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="6a65c-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
