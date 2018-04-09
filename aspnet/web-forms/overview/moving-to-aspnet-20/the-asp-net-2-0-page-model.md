---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: O modelo do ASP.NET 2.0 página | Microsoft Docs
author: microsoft
description: No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind. Por trás do código podem ser implementado usando ambos os attr Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="93584-104">O modelo do ASP.NET 2.0 de página</span><span class="sxs-lookup"><span data-stu-id="93584-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="93584-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="93584-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="93584-106">No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="93584-107">Por trás do código podem ser implementado usando o atributo Src ou o atributo CodeBehind do @Page diretiva.</span><span class="sxs-lookup"><span data-stu-id="93584-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="93584-108">No ASP.NET 2.0, os desenvolvedores ainda pode escolher entre o código embutido e code-behind, mas houve aprimoramentos significativos para o modelo de code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="93584-109">No ASP.NET 1. x, os desenvolvedores precisavam escolher entre um modelo de código em linha e um modelo de código code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="93584-110">Por trás do código podem ser implementado usando o atributo Src ou o atributo CodeBehind do @Page diretiva.</span><span class="sxs-lookup"><span data-stu-id="93584-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="93584-111">No ASP.NET 2.0, os desenvolvedores ainda pode escolher entre o código embutido e code-behind, mas houve aprimoramentos significativos para o modelo de code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="93584-112">Melhorias no modelo de Code-Behind</span><span class="sxs-lookup"><span data-stu-id="93584-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="93584-113">Para compreender totalmente as alterações no modelo de code-behind no ASP.NET 2.0, o melhor desempenho para analisar rapidamente o modelo como ele existia no ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="93584-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="93584-114">O modelo de Code-Behind no ASP.NET 1. x</span><span class="sxs-lookup"><span data-stu-id="93584-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="93584-115">No ASP.NET 1. x, o modelo de code-behind consistia em um arquivo ASPX (o formulário da Web) e um arquivo de code-behind que contém o código de programação.</span><span class="sxs-lookup"><span data-stu-id="93584-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="93584-116">Os dois arquivos foram conectados usando o @Page diretiva no arquivo ASPX.</span><span class="sxs-lookup"><span data-stu-id="93584-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="93584-117">Cada controle na página. ASPX possui uma declaração correspondente no arquivo code-behind como uma variável de instância.</span><span class="sxs-lookup"><span data-stu-id="93584-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="93584-118">O arquivo code-behind também contivesse um código de associação de evento e gerou o código necessário para o designer do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93584-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="93584-119">Esse modelo funcionou muito bem, mas porque cada elemento ASP.NET na página ASPX necessário código correspondente no arquivo code-behind, não havia nenhum verdadeira separação de código e conteúdo.</span><span class="sxs-lookup"><span data-stu-id="93584-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="93584-120">Por exemplo, se um designer adicionado um novo controle de servidor para um arquivo ASPX fora do IDE do Visual Studio, o aplicativo interrompe devido à ausência de uma declaração para o controle no arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="93584-121">O modelo de Code-Behind no ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="93584-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="93584-122">O ASP.NET 2.0 melhora significativamente esse modelo.</span><span class="sxs-lookup"><span data-stu-id="93584-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="93584-123">No ASP.NET 2.0, por trás do código é implementado usando o novo *classes parciais* fornecido no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="93584-124">A classe code-behind no ASP.NET 2.0 está definidos como uma classe parcial, que significa que ele contém apenas uma parte da definição de classe.</span><span class="sxs-lookup"><span data-stu-id="93584-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="93584-125">A parte restante da definição de classe é gerada dinamicamente pelo ASP.NET 2.0 usando a página ASPX em tempo de execução ou quando o site da Web é pré-compilado.</span><span class="sxs-lookup"><span data-stu-id="93584-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="93584-126">O vínculo entre o arquivo code-behind e a página ASPX ainda será estabelecido usando a diretiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="93584-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="93584-127">No entanto, em vez de um atributo Src ou de code-behind, o ASP.NET 2.0 agora usa o atributo CodeFile.</span><span class="sxs-lookup"><span data-stu-id="93584-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="93584-128">O atributo Inherits também é usado para especificar o nome da classe para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="93584-129">Uma diretiva de página @ típica pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="93584-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="93584-130">Uma definição de classe comum em um arquivo de code-behind ASP.NET 2.0 pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="93584-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="93584-131">C# e Visual Basic são as únicas linguagens gerenciadas que oferecem suporte a classes parciais no momento.</span><span class="sxs-lookup"><span data-stu-id="93584-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="93584-132">Portanto, os desenvolvedores que usam j# não poderá usar o modelo de code-behind em ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="93584-133">O novo modelo aprimora o modelo de code-behind porque os desenvolvedores agora têm arquivos de código que contêm apenas o código que ele criou.</span><span class="sxs-lookup"><span data-stu-id="93584-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="93584-134">Ele também fornece uma separação true de código e conteúdo porque não há nenhum declarações de variável de instância no arquivo code-behind.</span><span class="sxs-lookup"><span data-stu-id="93584-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="93584-135">Porque a classe parcial para a página ASPX é onde a associação de evento ocorre, os desenvolvedores do Visual Basic podem perceber um aumento pequeno de desempenho usando a palavra-chave Handles no code-behind para associar a eventos.</span><span class="sxs-lookup"><span data-stu-id="93584-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="93584-136">C# não tem nenhuma palavra-chave equivalente.</span><span class="sxs-lookup"><span data-stu-id="93584-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="93584-137">Novos atributos de diretiva de página @</span><span class="sxs-lookup"><span data-stu-id="93584-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="93584-138">O ASP.NET 2.0 adiciona muitos novos atributos para a diretiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="93584-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="93584-139">Os atributos a seguir são novos no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="93584-140">Async</span><span class="sxs-lookup"><span data-stu-id="93584-140">Async</span></span>

<span data-ttu-id="93584-141">O atributo Async permite configurar página a ser executada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="93584-142">Também abrange páginas assíncronas neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="93584-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="93584-143">AsyncTimeout</span></span>

<span data-ttu-id="93584-144">Especificar o tempo limite para as páginas assíncronas.</span><span class="sxs-lookup"><span data-stu-id="93584-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="93584-145">O padrão é 45 segundos.</span><span class="sxs-lookup"><span data-stu-id="93584-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="93584-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="93584-146">CodeFile</span></span>

<span data-ttu-id="93584-147">O atributo CodeFile é a substituição para o atributo CodeBehind no Visual Studio 2002/2003.</span><span class="sxs-lookup"><span data-stu-id="93584-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="93584-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="93584-148">CodeFileBaseClass</span></span>

<span data-ttu-id="93584-149">O atributo CodeFileBaseClass é usado em casos onde você deseja várias páginas para derivar de uma única classe base.</span><span class="sxs-lookup"><span data-stu-id="93584-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="93584-150">Devido a implementação de classes parciais no ASP.NET, sem esse atributo, de uma classe base que usa campos comuns compartilhados para fazer referência a controles declarados em uma página ASPX não funcionará corretamente porque ASP. Mecanismo de compilação de redes criará automaticamente os novos membros com base nos controles na página.</span><span class="sxs-lookup"><span data-stu-id="93584-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="93584-151">Portanto, se você quiser uma classe base comum para duas ou mais páginas no ASP.NET, você precisará definir especifique sua classe base no atributo ' CodeFileBaseClass e, em seguida, derive cada classe de páginas de classe base.</span><span class="sxs-lookup"><span data-stu-id="93584-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="93584-152">O atributo CodeFile também é necessário quando este atributo é usado.</span><span class="sxs-lookup"><span data-stu-id="93584-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="93584-153">CompilationMode</span><span class="sxs-lookup"><span data-stu-id="93584-153">CompilationMode</span></span>

<span data-ttu-id="93584-154">Este atributo permite que você defina a propriedade CompilationMode da página ASPX.</span><span class="sxs-lookup"><span data-stu-id="93584-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="93584-155">A propriedade CompilationMode é uma enumeração que contém os valores **sempre**, **automática**, e **nunca**.</span><span class="sxs-lookup"><span data-stu-id="93584-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="93584-156">O padrão é **sempre**.</span><span class="sxs-lookup"><span data-stu-id="93584-156">The default is **Always**.</span></span> <span data-ttu-id="93584-157">O **automática** configuração impedirá ASP.NET dinamicamente a página de compilação se possível.</span><span class="sxs-lookup"><span data-stu-id="93584-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="93584-158">Excluindo páginas de compilação dinâmica aumenta o desempenho.</span><span class="sxs-lookup"><span data-stu-id="93584-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="93584-159">No entanto, se uma página que foi excluída contém código que deve ser compilado, um erro será gerado quando a página é acessada.</span><span class="sxs-lookup"><span data-stu-id="93584-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="93584-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="93584-160">EnableEventValidation</span></span>

<span data-ttu-id="93584-161">Esse atributo especifica se eventos de postback e retorno de chamada são validados.</span><span class="sxs-lookup"><span data-stu-id="93584-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="93584-162">Quando habilitada, argumentos de postback ou de eventos de retorno de chamada são verificados para garantir que eles foram originados o controle de servidor que originalmente os processou.</span><span class="sxs-lookup"><span data-stu-id="93584-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="93584-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="93584-163">EnableTheming</span></span>

<span data-ttu-id="93584-164">Esse atributo especifica se ou não os temas do ASP.NET são usados em uma página.</span><span class="sxs-lookup"><span data-stu-id="93584-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="93584-165">O padrão é **false**.</span><span class="sxs-lookup"><span data-stu-id="93584-165">The default is **false**.</span></span> <span data-ttu-id="93584-166">Temas do ASP.NET são abordados em [módulo 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="93584-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="93584-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="93584-167">LinePragmas</span></span>

<span data-ttu-id="93584-168">Esse atributo especifica se pragmas de linha devem ser adicionados durante a compilação.</span><span class="sxs-lookup"><span data-stu-id="93584-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="93584-169">Pragmas de linha são opções usadas pelo depuradores para marcar seções específicas de código.</span><span class="sxs-lookup"><span data-stu-id="93584-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="93584-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="93584-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="93584-171">Esse atributo especifica se JavaScript é injetado para a página para manter a posição de rolagem entre postbacks.</span><span class="sxs-lookup"><span data-stu-id="93584-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="93584-172">Esse atributo é **false** por padrão.</span><span class="sxs-lookup"><span data-stu-id="93584-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="93584-173">Quando esse atributo é **true**, ASP.NET adicionará um &lt;script&gt; bloco em um postback tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="93584-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="93584-174">Observe que a origem para este bloco de script é WebResource.</span><span class="sxs-lookup"><span data-stu-id="93584-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="93584-175">Este recurso não é um caminho físico.</span><span class="sxs-lookup"><span data-stu-id="93584-175">This resource is not a physical path.</span></span> <span data-ttu-id="93584-176">Quando esse script é solicitado, o ASP.NET cria dinamicamente o script.</span><span class="sxs-lookup"><span data-stu-id="93584-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="93584-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="93584-177">MasterPageFile</span></span>

<span data-ttu-id="93584-178">Esse atributo especifica o arquivo de página mestra para a página atual.</span><span class="sxs-lookup"><span data-stu-id="93584-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="93584-179">O caminho pode ser relativo ou absoluto.</span><span class="sxs-lookup"><span data-stu-id="93584-179">The path can be relative or absolute.</span></span> <span data-ttu-id="93584-180">Páginas mestras são abordadas em [módulo 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="93584-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="93584-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="93584-181">StyleSheetTheme</span></span>

<span data-ttu-id="93584-182">Este atributo permite que você substitua a propriedades de aparência da interface do usuário definidas por um tema ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="93584-183">Temas são abordados em [módulo 10](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="93584-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="93584-184">{1&gt;Tema&lt;1}</span><span class="sxs-lookup"><span data-stu-id="93584-184">Theme</span></span>

<span data-ttu-id="93584-185">Especifica o tema da página.</span><span class="sxs-lookup"><span data-stu-id="93584-185">Specifies the theme for the page.</span></span> <span data-ttu-id="93584-186">Se um valor não for especificado para o atributo StyleSheetTheme, o atributo tema substitui todos os estilos aplicados aos controles na página.</span><span class="sxs-lookup"><span data-stu-id="93584-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="93584-187">Título</span><span class="sxs-lookup"><span data-stu-id="93584-187">Title</span></span>

<span data-ttu-id="93584-188">Define o título da página.</span><span class="sxs-lookup"><span data-stu-id="93584-188">Sets the title for the page.</span></span> <span data-ttu-id="93584-189">O valor especificado aqui aparecerão no &lt;título&gt; elemento da página renderizada.</span><span class="sxs-lookup"><span data-stu-id="93584-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="93584-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="93584-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="93584-191">Define o valor de enumeração ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="93584-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="93584-192">Os valores disponíveis são **sempre**, **automática**, e **nunca**.</span><span class="sxs-lookup"><span data-stu-id="93584-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="93584-193">O valor padrão é **automática**. Quando esse atributo é definido como um valor de **automática**, viewstate está criptografado é um controle solicitá-lo chamando o **RegisterRequiresViewStateEncryption** método.</span><span class="sxs-lookup"><span data-stu-id="93584-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="93584-194">Definir valores de propriedade pública via @ diretiva de página</span><span class="sxs-lookup"><span data-stu-id="93584-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="93584-195">Outro novo recurso da diretiva @ Page no ASP.NET 2.0 é a capacidade de definir o valor inicial de propriedades públicas de uma classe base.</span><span class="sxs-lookup"><span data-stu-id="93584-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="93584-196">Suponha que, por exemplo, você tem uma propriedade pública denominada **SomeText** em sua classe base e você gostaria que ele seja inicializada para **Hello** quando uma página for carregada.</span><span class="sxs-lookup"><span data-stu-id="93584-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="93584-197">Você pode fazer isso definindo o valor simplesmente na diretiva de página @ da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="93584-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="93584-198">O **SomeText** atributo da diretiva @ Page define o valor inicial da propriedade SomeText na classe base para *Olá!*.</span><span class="sxs-lookup"><span data-stu-id="93584-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="93584-199">O vídeo abaixo está um passo a passo de configuração do valor inicial de uma propriedade pública em uma classe base usando a diretiva @ Page.</span><span class="sxs-lookup"><span data-stu-id="93584-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="93584-200">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="93584-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="93584-201">Novas propriedades públicas da classe de página</span><span class="sxs-lookup"><span data-stu-id="93584-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="93584-202">As propriedades públicas a seguir são novas no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="93584-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="93584-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="93584-204">Retorna o caminho relativo de aplicativo para a página ou controle.</span><span class="sxs-lookup"><span data-stu-id="93584-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="93584-205">Por exemplo, para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~ / pasta.</span><span class="sxs-lookup"><span data-stu-id="93584-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="93584-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="93584-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="93584-207">Retorna o caminho relativo para a página ou controle.</span><span class="sxs-lookup"><span data-stu-id="93584-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="93584-208">Por exemplo, para uma página localizada em http://app/folder/page.aspx, a propriedade retorna ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="93584-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="93584-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="93584-209">AsyncTimeout</span></span>

<span data-ttu-id="93584-210">Obtém ou define o limite usado para manipulação de página assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="93584-211">(Páginas assíncronas serão abordadas neste módulo.)</span><span class="sxs-lookup"><span data-stu-id="93584-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="93584-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="93584-212">ClientQueryString</span></span>

<span data-ttu-id="93584-213">Uma propriedade somente leitura que retorna a parte da cadeia de caracteres de consulta da URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="93584-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="93584-214">Esse valor é codificada de URL.</span><span class="sxs-lookup"><span data-stu-id="93584-214">This value is URL encoded.</span></span> <span data-ttu-id="93584-215">Você pode usar o método UrlDecode da classe HttpServerUtility para decodificá-lo.</span><span class="sxs-lookup"><span data-stu-id="93584-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="93584-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="93584-216">ClientScript</span></span>

<span data-ttu-id="93584-217">Essa propriedade retorna um objeto de ClientScriptManager que pode ser usado para gerenciar a emissão de ASP.NETs de script do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="93584-218">(A classe ClientScriptManager é abordada neste módulo).</span><span class="sxs-lookup"><span data-stu-id="93584-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="93584-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="93584-219">EnableEventValidation</span></span>

<span data-ttu-id="93584-220">Essa propriedade controla se validação do evento está habilitada para eventos de postback e retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="93584-221">Quando habilitada, os argumentos de postback ou de eventos de retorno de chamada são verificados para garantir que eles foram originados o controle de servidor que originalmente os processou.</span><span class="sxs-lookup"><span data-stu-id="93584-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="93584-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="93584-222">EnableTheming</span></span>

<span data-ttu-id="93584-223">Essa propriedade obtém ou define um valor booleano que especifica se ou não um tema ASP.NET 2.0 aplica-se à página.</span><span class="sxs-lookup"><span data-stu-id="93584-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="93584-224">Formulário</span><span class="sxs-lookup"><span data-stu-id="93584-224">Form</span></span>

<span data-ttu-id="93584-225">Essa propriedade retorna o formulário HTML na página ASPX como um objeto HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="93584-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="93584-226">Cabeçalho</span><span class="sxs-lookup"><span data-stu-id="93584-226">Header</span></span>

<span data-ttu-id="93584-227">Essa propriedade retorna uma referência a um objeto de HtmlHead que contém o cabeçalho da página.</span><span class="sxs-lookup"><span data-stu-id="93584-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="93584-228">Você pode usar o objeto retornado de HtmlHead para get/set folhas de estilo, marcas Meta, etc.</span><span class="sxs-lookup"><span data-stu-id="93584-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="93584-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="93584-229">IdSeparator</span></span>

<span data-ttu-id="93584-230">Essa propriedade somente leitura obtém o caractere que é usado para separar os identificadores de controle quando o ASP.NET está construindo uma ID exclusiva para controles em uma página.</span><span class="sxs-lookup"><span data-stu-id="93584-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="93584-231">Ele não se destina a ser usado diretamente no seu código.</span><span class="sxs-lookup"><span data-stu-id="93584-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="93584-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="93584-232">IsAsync</span></span>

<span data-ttu-id="93584-233">Essa propriedade permite páginas assíncronas.</span><span class="sxs-lookup"><span data-stu-id="93584-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="93584-234">Páginas assíncronas são discutidas neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="93584-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="93584-235">IsCallback</span></span>

<span data-ttu-id="93584-236">Essa propriedade somente leitura retorna **true** se a página é o resultado de um retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="93584-237">Retornos de chamada são discutidos neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="93584-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="93584-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="93584-239">Essa propriedade somente leitura retorna **true** se a página for parte de uma postagem entre páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="93584-240">Várias páginas são abordadas neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="93584-241">Itens</span><span class="sxs-lookup"><span data-stu-id="93584-241">Items</span></span>

<span data-ttu-id="93584-242">Retorna uma referência a uma instância de IDictionary que contém todos os objetos armazenados no contexto de páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="93584-243">Você pode adicionar itens a esse objeto IDictionary e eles estarão disponíveis para você durante o tempo de vida do contexto.</span><span class="sxs-lookup"><span data-stu-id="93584-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="93584-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="93584-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="93584-245">Essa propriedade controla se o ASP.NET emite JavaScript que mantém que as páginas de posição de rolagem no navegador após a ocorrência de um postback.</span><span class="sxs-lookup"><span data-stu-id="93584-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="93584-246">(Detalhes dessa propriedade foram discutidos neste módulo).</span><span class="sxs-lookup"><span data-stu-id="93584-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="93584-247">mestre</span><span class="sxs-lookup"><span data-stu-id="93584-247">Master</span></span>

<span data-ttu-id="93584-248">Essa propriedade somente leitura retorna uma referência à instância do MasterPage para uma página para a qual uma página mestra foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="93584-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="93584-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="93584-249">MasterPageFile</span></span>

<span data-ttu-id="93584-250">Obtém ou define o nome do arquivo de página mestra para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="93584-251">Essa propriedade só pode ser definida no método PreInit.</span><span class="sxs-lookup"><span data-stu-id="93584-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="93584-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="93584-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="93584-253">Esta propriedade obtém ou define o comprimento máximo para o estado de páginas em bytes.</span><span class="sxs-lookup"><span data-stu-id="93584-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="93584-254">Se a propriedade é definida como um número positivo, o estado de exibição de páginas será ser dividido em vários campos ocultos para que ele não excede o número de bytes especificado.</span><span class="sxs-lookup"><span data-stu-id="93584-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="93584-255">Se a propriedade for um número negativo, o estado de exibição não será dividido em partes.</span><span class="sxs-lookup"><span data-stu-id="93584-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="93584-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="93584-256">PageAdapter</span></span>

<span data-ttu-id="93584-257">Retorna uma referência ao objeto PageAdapter que modifica a página para o navegador solicitante.</span><span class="sxs-lookup"><span data-stu-id="93584-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="93584-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="93584-258">PreviousPage</span></span>

<span data-ttu-id="93584-259">Retorna uma referência à página anterior em caso de uma transferência de servidor ou um postback da página.</span><span class="sxs-lookup"><span data-stu-id="93584-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="93584-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="93584-260">SkinID</span></span>

<span data-ttu-id="93584-261">Especifica a capa do ASP.NET 2.0 para aplicar à página.</span><span class="sxs-lookup"><span data-stu-id="93584-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="93584-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="93584-262">StyleSheetTheme</span></span>

<span data-ttu-id="93584-263">Essa propriedade obtém ou define a folha de estilo que é aplicada a uma página.</span><span class="sxs-lookup"><span data-stu-id="93584-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="93584-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="93584-264">TemplateControl</span></span>

<span data-ttu-id="93584-265">Retorna uma referência para o controle recipiente para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="93584-266">{1&gt;Tema&lt;1}</span><span class="sxs-lookup"><span data-stu-id="93584-266">Theme</span></span>

<span data-ttu-id="93584-267">Obtém ou define o nome do tema ASP.NET 2.0 aplicado à página.</span><span class="sxs-lookup"><span data-stu-id="93584-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="93584-268">Esse valor deve ser definido antes do método PreInit.</span><span class="sxs-lookup"><span data-stu-id="93584-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="93584-269">Título</span><span class="sxs-lookup"><span data-stu-id="93584-269">Title</span></span>

<span data-ttu-id="93584-270">Essa propriedade obtém ou define o título da página como obtido do cabeçalho de páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="93584-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="93584-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="93584-272">Obtém ou define o ViewStateEncryptionMode da página.</span><span class="sxs-lookup"><span data-stu-id="93584-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="93584-273">Consulte uma discussão detalhada sobre essa propriedade neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="93584-274">Novas propriedades protegidas de classe de página</span><span class="sxs-lookup"><span data-stu-id="93584-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="93584-275">A seguir estão as novas propriedades protegidas da classe de página no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="93584-276">Adaptador</span><span class="sxs-lookup"><span data-stu-id="93584-276">Adapter</span></span>

<span data-ttu-id="93584-277">Retorna uma referência para o ControlAdapter que renderiza a página no dispositivo que solicitou que ela.</span><span class="sxs-lookup"><span data-stu-id="93584-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="93584-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="93584-278">AsyncMode</span></span>

<span data-ttu-id="93584-279">Essa propriedade indica se a página é processada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="93584-280">Ele é destinado ao uso pelo tempo de execução e não diretamente no código.</span><span class="sxs-lookup"><span data-stu-id="93584-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="93584-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="93584-281">ClientIDSeparator</span></span>

<span data-ttu-id="93584-282">Essa propriedade retorna o caractere usado como separador ao criar IDs de cliente exclusivos para controles.</span><span class="sxs-lookup"><span data-stu-id="93584-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="93584-283">Ele é destinado ao uso pelo tempo de execução e não diretamente no código.</span><span class="sxs-lookup"><span data-stu-id="93584-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="93584-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="93584-284">PageStatePersister</span></span>

<span data-ttu-id="93584-285">Essa propriedade retorna o objeto PageStatePersister para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="93584-286">Essa propriedade é usada principalmente por desenvolvedores de controle do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="93584-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="93584-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="93584-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="93584-288">Essa propriedade retorna um suffic exclusivo que é acrescentado ao caminho do arquivo de cache de navegadores.</span><span class="sxs-lookup"><span data-stu-id="93584-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="93584-289">O valor padrão é \_ \_ufps = e um número de 6 dígitos.</span><span class="sxs-lookup"><span data-stu-id="93584-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="93584-290">Novos métodos públicos da classe de página</span><span class="sxs-lookup"><span data-stu-id="93584-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="93584-291">Os métodos públicos a seguir são novos para a classe de página no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="93584-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="93584-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="93584-293">Este método registra delegados de manipulador de eventos para execução da página assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="93584-294">Páginas assíncronas são discutidas neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="93584-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="93584-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="93584-296">Aplica-se as propriedades em uma folha de estilos de páginas para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="93584-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="93584-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="93584-298">Seres esse método uma tarefa assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="93584-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="93584-299">GetValidators</span></span>

<span data-ttu-id="93584-300">Retorna uma coleção de validadores para o grupo de validação especificada ou o grupo de validação padrão se nenhum for especificado.</span><span class="sxs-lookup"><span data-stu-id="93584-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="93584-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="93584-301">RegisterAsyncTask</span></span>

<span data-ttu-id="93584-302">Este método registra uma nova tarefa assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-302">This method registers a new async task.</span></span> <span data-ttu-id="93584-303">Páginas assíncronas são abordadas neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="93584-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="93584-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="93584-305">Esse método informa ao ASP.NET que o estado de controle de páginas deve ser persistente.</span><span class="sxs-lookup"><span data-stu-id="93584-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="93584-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="93584-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="93584-307">Esse método informa ao ASP.NET que o viewstate páginas requer criptografia.</span><span class="sxs-lookup"><span data-stu-id="93584-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="93584-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="93584-308">ResolveClientUrl</span></span>

<span data-ttu-id="93584-309">Retorna uma URL relativa que pode ser usada para solicitações de cliente para imagens, etc.</span><span class="sxs-lookup"><span data-stu-id="93584-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="93584-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="93584-310">SetFocus</span></span>

<span data-ttu-id="93584-311">Esse método definirá o foco para o controle que é especificado quando a página for carregada inicialmente.</span><span class="sxs-lookup"><span data-stu-id="93584-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="93584-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="93584-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="93584-313">Esse método irá cancelar o registro de controle que é passado como não há mais a necessidade de persistência de estado do controle.</span><span class="sxs-lookup"><span data-stu-id="93584-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="93584-314">Alterações para o ciclo de vida da página</span><span class="sxs-lookup"><span data-stu-id="93584-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="93584-315">O ciclo de vida da página no ASP.NET 2.0 não mudou significativamente, mas há alguns novos métodos que você deve estar atento.</span><span class="sxs-lookup"><span data-stu-id="93584-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="93584-316">O ciclo de vida de página do ASP.NET 2.0 é descrito a seguir.</span><span class="sxs-lookup"><span data-stu-id="93584-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="93584-317">PreInit (novo no ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="93584-318">O evento PreInit é o mais antigo estágio do ciclo de vida que um desenvolvedor pode acessar.</span><span class="sxs-lookup"><span data-stu-id="93584-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="93584-319">A adição desse evento torna possível alterar temas ASP.NET 2.0, páginas mestras programaticamente, acessar as propriedades de um perfil do ASP.NET 2.0, etc. Se você estiver em um estado de postback, importante perceber Viewstate ainda não foram aplicada aos controles neste ponto do ciclo de vida.</span><span class="sxs-lookup"><span data-stu-id="93584-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="93584-320">Portanto, se um desenvolvedor de uma propriedade de um controle é alterado neste estágio, ele provavelmente será substituído posteriormente no ciclo de vida de páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="93584-321">Init</span><span class="sxs-lookup"><span data-stu-id="93584-321">Init</span></span>

<span data-ttu-id="93584-322">O evento Init não mudou de ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="93584-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="93584-323">Isso é onde você deseja ler ou inicializar propriedades de controles na página.</span><span class="sxs-lookup"><span data-stu-id="93584-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="93584-324">Neste estágio, páginas mestras, temas, etc. já estiverem aplicados para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="93584-325">InitComplete (novo no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="93584-326">O evento InitComplete é chamado no final do estágio de inicialização de páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="93584-327">Neste ponto do ciclo de vida, você pode acessar controles na página, mas seu estado ainda não foi populado.</span><span class="sxs-lookup"><span data-stu-id="93584-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="93584-328">Pré-carregamento (nova no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="93584-329">Este evento é chamado após ter sido aplicada a todos os dados de postagem e apenas antes da página\_carga.</span><span class="sxs-lookup"><span data-stu-id="93584-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="93584-330">Carregamento</span><span class="sxs-lookup"><span data-stu-id="93584-330">Load</span></span>

<span data-ttu-id="93584-331">O evento de carregamento não foi alterado de ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="93584-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="93584-332">LoadComplete (novo no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="93584-333">O evento LoadComplete é o último evento no estágio de carregamento de páginas.</span><span class="sxs-lookup"><span data-stu-id="93584-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="93584-334">Neste estágio, todos os dados de postagem e viewstate foi aplicado à página.</span><span class="sxs-lookup"><span data-stu-id="93584-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="93584-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="93584-335">PreRender</span></span>

<span data-ttu-id="93584-336">Se desejar que o viewstate ser mantidas corretamente para controles que são adicionados dinamicamente para a página do evento PreRender é a última oportunidade para adicioná-los.</span><span class="sxs-lookup"><span data-stu-id="93584-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="93584-337">PreRenderComplete (novo no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="93584-338">No estágio PreRenderComplete, todos os controles foram adicionados para a página e a página está pronta para ser processado.</span><span class="sxs-lookup"><span data-stu-id="93584-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="93584-339">O evento PreRenderComplete é o último evento gerado antes do viewstate de páginas é salvo.</span><span class="sxs-lookup"><span data-stu-id="93584-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="93584-340">SaveStateComplete (novo no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="93584-341">O evento SaveStateComplete é chamado imediatamente depois que todos os estados de viewstate e controle de página foi salvo.</span><span class="sxs-lookup"><span data-stu-id="93584-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="93584-342">Este é o último evento antes da página, na verdade, é renderizada no navegador.</span><span class="sxs-lookup"><span data-stu-id="93584-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="93584-343">Renderizar</span><span class="sxs-lookup"><span data-stu-id="93584-343">Render</span></span>

<span data-ttu-id="93584-344">O método de processamento não foi alterado desde o ASP.NET 1. x.</span><span class="sxs-lookup"><span data-stu-id="93584-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="93584-345">Isso é onde o HtmlTextWriter é inicializado e a página é renderizada no navegador.</span><span class="sxs-lookup"><span data-stu-id="93584-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="93584-346">Postback da página no ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="93584-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="93584-347">No ASP.NET 1. x, postbacks precisavam post para a mesma página.</span><span class="sxs-lookup"><span data-stu-id="93584-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="93584-348">Várias páginas não eram permitidas.</span><span class="sxs-lookup"><span data-stu-id="93584-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="93584-349">O ASP.NET 2.0 adiciona a capacidade de volta para uma página diferente por meio da interface IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="93584-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="93584-350">Qualquer controle que implementa a interface IButtonControl novo (botão, LinkButton e ImageButton além de controles personalizados de terceiros) pode aproveitar essa nova funcionalidade com o uso do atributo PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="93584-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="93584-351">O código a seguir mostra um controle de botão que envia de volta para uma segunda página.</span><span class="sxs-lookup"><span data-stu-id="93584-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="93584-352">Quando a página é enviada de volta, a página que inicia a postagem é acessível por meio da propriedade PreviousPage na segunda página.</span><span class="sxs-lookup"><span data-stu-id="93584-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="93584-353">Essa funcionalidade é implementada por meio do novo formulário da Web\_DoPostBackWithOptions função de cliente que o ASP.NET 2.0 processa para a página quando um controle envia de volta para uma página diferente.</span><span class="sxs-lookup"><span data-stu-id="93584-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="93584-354">Esta função JavaScript é fornecida pelo manipulador WebResource novo que emite o script para o cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="93584-355">O vídeo abaixo está um passo a passo de um postback da página.</span><span class="sxs-lookup"><span data-stu-id="93584-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="93584-356">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="93584-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="93584-357">Para obter mais detalhes em várias páginas</span><span class="sxs-lookup"><span data-stu-id="93584-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="93584-358">ViewState</span><span class="sxs-lookup"><span data-stu-id="93584-358">Viewstate</span></span>

<span data-ttu-id="93584-359">Você pode ter solicitado por conta própria já sobre o que acontece com o viewstate desde a primeira página em um cenário de postback da página.</span><span class="sxs-lookup"><span data-stu-id="93584-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="93584-360">Afinal, qualquer controle que não implementam IPostBackDataHandler persistirá seu estado por meio de viewstate, então para ter acesso às propriedades de controle na segunda página de uma postagem entre páginas, você deve ter acesso para o viewstate para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="93584-361">O ASP.NET 2.0 cuida desse cenário usando um novo campo oculto na segunda página chamado \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="93584-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="93584-362">O \_ \_campo de formulário PREVIOUSPAGE contém o viewstate para a primeira página para que você possa ter acesso às propriedades de todos os controles na segunda página.</span><span class="sxs-lookup"><span data-stu-id="93584-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="93584-363">Evitar FindControl</span><span class="sxs-lookup"><span data-stu-id="93584-363">Circumventing FindControl</span></span>

<span data-ttu-id="93584-364">Vídeo passo a passo de uma postagem entre páginas, usei o método FindControl para obter uma referência para o controle de caixa de texto na primeira página.</span><span class="sxs-lookup"><span data-stu-id="93584-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="93584-365">Esse método funciona bem para essa finalidade, mas FindControl é caro e precisar gravar código adicional.</span><span class="sxs-lookup"><span data-stu-id="93584-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="93584-366">Felizmente, o ASP.NET 2.0 fornece uma alternativa para FindControl para essa finalidade que funcionarão em vários cenários.</span><span class="sxs-lookup"><span data-stu-id="93584-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="93584-367">A diretiva PreviousPageType permite que você tenha uma referência fortemente tipada para a página anterior usando o atributo VirtualPath ou o TypeName.</span><span class="sxs-lookup"><span data-stu-id="93584-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="93584-368">O atributo TypeName permite que você especifique o tipo de página anterior, enquanto o atributo VirtualPath permite que você se refira à página anterior usando um caminho virtual.</span><span class="sxs-lookup"><span data-stu-id="93584-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="93584-369">Depois de configurar a diretiva PreviousPageType, em seguida, você deve expor a controles, etc. para o qual você deseja permitir o acesso usando as propriedades públicas.</span><span class="sxs-lookup"><span data-stu-id="93584-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="93584-370">Postagem entre páginas de laboratório 1</span><span class="sxs-lookup"><span data-stu-id="93584-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="93584-371">Neste laboratório, você criará um aplicativo que usa a nova funcionalidade de postagem entre páginas do ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="93584-372">Abra o Visual Studio 2005 e crie um novo site da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="93584-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="93584-373">Adicione um novo formulário da Web chamado Page2. aspx.</span><span class="sxs-lookup"><span data-stu-id="93584-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="93584-374">Abra o Default.aspx no modo Design e adicione um controle de botão e um controle de caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="93584-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="93584-375">Dê o controle de botão de uma ID de **SubmitButton** e a caixa de texto de uma ID de controle **nome de usuário**.</span><span class="sxs-lookup"><span data-stu-id="93584-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="93584-376">Defina a propriedade de PostBackUrl do botão como Page2. aspx.</span><span class="sxs-lookup"><span data-stu-id="93584-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="93584-377">Abra Page2. aspx no modo de exibição de origem.</span><span class="sxs-lookup"><span data-stu-id="93584-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="93584-378">Adicione uma diretiva @ PreviousPageType conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="93584-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="93584-379">Adicione o seguinte código para a página\_carga do code-behind do Page2. aspx:</span><span class="sxs-lookup"><span data-stu-id="93584-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="93584-380">Compile o projeto clicando na compilação no menu compilar.</span><span class="sxs-lookup"><span data-stu-id="93584-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="93584-381">Adicione o seguinte código para code-behind para Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="93584-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="93584-382">Altere a página\_Load em Page2. aspx ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="93584-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="93584-383">Compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="93584-383">Build the project.</span></span>
11. <span data-ttu-id="93584-384">Execute o projeto.</span><span class="sxs-lookup"><span data-stu-id="93584-384">Run the project.</span></span>
12. <span data-ttu-id="93584-385">Digite seu nome na caixa de texto e clique no botão.</span><span class="sxs-lookup"><span data-stu-id="93584-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="93584-386">O que é o resultado?</span><span class="sxs-lookup"><span data-stu-id="93584-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="93584-387">Páginas assíncronas no ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="93584-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="93584-388">Muitos problemas de contenção no ASP.NET são causados por latência de chamadas externas (como chamadas de banco de dados ou serviço da Web), latência de e/s de arquivo, etc. Quando uma solicitação é feita em relação a um aplicativo ASP.NET, o ASP.NET usa um dos seus threads de trabalho para que a solicitação de serviço.</span><span class="sxs-lookup"><span data-stu-id="93584-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="93584-389">Essa solicitação é responsável por esse thread até que a solicitação é concluída e a resposta foi enviada.</span><span class="sxs-lookup"><span data-stu-id="93584-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="93584-390">ASP.NET 2.0 tenta resolver problemas de latência com esses tipos de problemas, adicionando a capacidade de executar as páginas de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="93584-391">Isso significa que um thread de trabalho pode iniciar a solicitação e, em seguida, entregar execução adicionais para outro thread, retornando, assim, ao pool de threads disponíveis rapidamente.</span><span class="sxs-lookup"><span data-stu-id="93584-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="93584-392">Quando tiver concluído a e/s de arquivo, chamada de banco de dados, etc., um novo segmento é obtido do pool de threads para concluir a solicitação.</span><span class="sxs-lookup"><span data-stu-id="93584-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="93584-393">É a primeira etapa para fazer com que uma página executadas de forma assíncrona definir o **Async** atributo da diretiva de página da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="93584-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="93584-394">Esse atributo diz ao ASP.NET para implementar IHttpAsyncHandler para a página.</span><span class="sxs-lookup"><span data-stu-id="93584-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="93584-395">A próxima etapa é chamar o método AddOnPreRenderCompleteAsync em um ponto no ciclo de vida da página antes de PreRender.</span><span class="sxs-lookup"><span data-stu-id="93584-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="93584-396">(Normalmente, esse método é chamado na página\_carga.) O método AddOnPreRenderCompleteAsync utiliza dois parâmetros; um BeginEventHandler e um EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="93584-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="93584-397">O BeginEventHandler retorna um IAsyncResult, que é então passado como um parâmetro para o EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="93584-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="93584-398">O vídeo abaixo está um passo a passo de uma solicitação de página assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="93584-399">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="93584-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="93584-400">Uma página assíncrona não processa para o navegador até que o EndEventHandler seja concluída.</span><span class="sxs-lookup"><span data-stu-id="93584-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="93584-401">Sem dúvida, mas que alguns desenvolvedores serão considerado de solicitações assíncronas como sendo similares para retornos de chamada assíncrono.</span><span class="sxs-lookup"><span data-stu-id="93584-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="93584-402">É importante observar que não são.</span><span class="sxs-lookup"><span data-stu-id="93584-402">It's important to realize that they are not.</span></span> <span data-ttu-id="93584-403">O benefício para solicitações assíncronas é que o primeiro thread de trabalho pode ser retornado ao pool de threads para atender novas solicitações, reduzindo a contenção por estarem associados de e/s, etc.</span><span class="sxs-lookup"><span data-stu-id="93584-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="93584-404">Retornos de chamada de script no ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="93584-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="93584-405">Os desenvolvedores da Web sempre procuramos por maneiras de evitar a oscilação associada a um retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="93584-406">No ASP.NET 1. x, SmartNavigation foi o método mais comum para evitar a cintilação, mas SmartNavigation causou problemas para alguns desenvolvedores devido à complexidade de sua implementação no cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="93584-407">O ASP.NET 2.0 resolve esse problema com retornos de chamada de script.</span><span class="sxs-lookup"><span data-stu-id="93584-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="93584-408">Retornos de chamada de script utilizam XMLHttp para fazer solicitações ao servidor Web por meio de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="93584-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="93584-409">A solicitação XMLHttp retorna dados XML que, em seguida, podem ser manipulados por meio de DOM. do navegador</span><span class="sxs-lookup"><span data-stu-id="93584-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="93584-410">Código de XMLHttp é ocultado do usuário pelo novo manipulador WebResource.</span><span class="sxs-lookup"><span data-stu-id="93584-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="93584-411">Há várias etapas que são necessárias para configurar um retorno de chamada de script no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="93584-412">Etapa 1: Implementar a Interface ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="93584-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="93584-413">Em ordem para o ASP.NET reconhece sua página como participando de um retorno de chamada de script, você deve implementar a interface ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="93584-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="93584-414">Você pode fazer isso no seu arquivo code-behind da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="93584-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="93584-415">Você também pode fazer isso usando o tipo de diretiva @ implementa assim:</span><span class="sxs-lookup"><span data-stu-id="93584-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="93584-416">Normalmente você usaria a diretiva @ implementa ao usar o código ASP.NET embutido.</span><span class="sxs-lookup"><span data-stu-id="93584-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="93584-417">Etapa 2: Chamada GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="93584-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="93584-418">Conforme mencionado anteriormente, a chamada de XMLHttp é encapsulada no manipulador WebResource.</span><span class="sxs-lookup"><span data-stu-id="93584-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="93584-419">Quando a página é processada, o ASP.NET irá adicionar uma chamada para o formulário da Web\_DoCallback, um script de cliente que é fornecido pelo WebResource.</span><span class="sxs-lookup"><span data-stu-id="93584-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="93584-420">O formulário da Web\_DoCallback função substitui o \_ \_doPostBack função de retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="93584-421">Lembre-se de que \_ \_doPostBack programaticamente envia o formulário na página.</span><span class="sxs-lookup"><span data-stu-id="93584-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="93584-422">Em um cenário de retorno de chamada, você deseja impedir que um postback, portanto \_ \_doPostBack não será suficiente.</span><span class="sxs-lookup"><span data-stu-id="93584-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="93584-423">\_\_doPostBack ainda é processado para a página em um cenário de retorno de chamada de script de cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="93584-424">No entanto, ele não é usado para o retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="93584-425">Os argumentos para o formulário da Web\_DoCallback função do lado do cliente são fornecidos por meio da função de servidor GetCallbackEventReference que normalmente poderia ser chamado na página\_carga.</span><span class="sxs-lookup"><span data-stu-id="93584-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="93584-426">Uma chamada típica para GetCallbackEventReference pode ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="93584-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="93584-427">Nesse caso, cm é uma instância de ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="93584-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="93584-428">A classe ClientScriptManager será abordada neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="93584-429">Há várias versões sobrecarregadas GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="93584-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="93584-430">Nesse caso, os argumentos são da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="93584-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="93584-431">Uma referência para o controle onde GetCallbackEventReference está sendo chamado.</span><span class="sxs-lookup"><span data-stu-id="93584-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="93584-432">Nesse caso, é a página propriamente dita.</span><span class="sxs-lookup"><span data-stu-id="93584-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="93584-433">Um argumento de cadeia de caracteres que será passado do código do lado do cliente para o evento do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="93584-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="93584-434">Nesse caso, passando o valor de uma lista suspensa de Im chamado ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="93584-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="93584-435">O nome da função do lado do cliente que aceita o valor de retorno (como cadeia de caracteres) a partir do evento de retorno de chamada do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="93584-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="93584-436">Essa função será chamada apenas quando o retorno de chamada do lado do servidor foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="93584-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="93584-437">Portanto, para manter a eficiência, geralmente é recomendável usar a versão sobrecarregada de GetCallbackEventReference que usa um argumento de cadeia de caracteres adicionais especificando o nome de uma função de cliente para executar em caso de erro.</span><span class="sxs-lookup"><span data-stu-id="93584-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="93584-438">Uma cadeia de caracteres que representa uma função de cliente que iniciou antes do retorno de chamada para o servidor.</span><span class="sxs-lookup"><span data-stu-id="93584-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="93584-439">Nesse caso, não há nenhum desses scripts, para que o argumento for nulo.</span><span class="sxs-lookup"><span data-stu-id="93584-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="93584-440">Um booliano que especifica se deve ou não realizar o retorno de chamada de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="93584-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="93584-441">A chamada para o formulário da Web\_DoCallback no cliente passará esses argumentos.</span><span class="sxs-lookup"><span data-stu-id="93584-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="93584-442">Portanto, quando esta página é renderizada no cliente, esse código parecerá da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="93584-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="93584-443">Observe que a assinatura de função no cliente é um pouco diferente.</span><span class="sxs-lookup"><span data-stu-id="93584-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="93584-444">A função do lado do cliente passa 5 cadeias de caracteres e um valor booleano.</span><span class="sxs-lookup"><span data-stu-id="93584-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="93584-445">A cadeia de caracteres adicional (que é nula no exemplo acima) contém a função do lado do cliente que vai manipular os erros de retorno de chamada do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="93584-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="93584-446">Etapa 3: Conectar-se com o evento de controle do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="93584-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="93584-447">Observe que o valor de retorno de GetCallbackEventReference acima foi atribuído a uma variável de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="93584-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="93584-448">Essa cadeia de caracteres é usada para conectar-se com um evento no lado do cliente para o controle que inicia o retorno de chamada.</span><span class="sxs-lookup"><span data-stu-id="93584-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="93584-449">Neste exemplo, o retorno de chamada é iniciado por uma lista suspensa na página, portanto, gostaria de gancho de *OnChange* eventos.</span><span class="sxs-lookup"><span data-stu-id="93584-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="93584-450">Para conectar o evento no lado do cliente, basta adicione um manipulador para a marcação do lado do cliente da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="93584-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="93584-451">Lembre-se de que *cbRef* é o valor de retorno da chamada para GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="93584-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="93584-452">Ele contém a chamada para o formulário da Web\_DoCallback foi mostrada acima.</span><span class="sxs-lookup"><span data-stu-id="93584-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="93584-453">Etapa 4: Registrar o Script do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="93584-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="93584-454">Lembre-se que a chamada para GetCallbackEventReference especificado que um script de cliente chamado **ShowCompanyName** seria executado quando o retorno de chamada do lado do servidor é bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="93584-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="93584-455">Esse script deve ser adicionado à página usando uma instância ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="93584-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="93584-456">(A classe ClientScriptManager será dicussed neste módulo). Você pode fazer que like assim:</span><span class="sxs-lookup"><span data-stu-id="93584-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="93584-457">Etapa 5: Chamar os métodos da Interface ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="93584-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="93584-458">O ICallbackEventHandler contém dois métodos que você precisa implementar em seu código.</span><span class="sxs-lookup"><span data-stu-id="93584-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="93584-459">Eles são **RaiseCallbackEvent** e **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="93584-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="93584-460">**RaiseCallbackEvent** usa uma cadeia de caracteres como um argumento e retorna nada.</span><span class="sxs-lookup"><span data-stu-id="93584-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="93584-461">O argumento de cadeia de caracteres é passado da chamada do lado do cliente para o formulário da Web\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="93584-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="93584-462">Nesse caso, esse valor é o *valor* atributo da lista suspensa chamado ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="93584-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="93584-463">O código do lado do servidor deve ser colocado no método RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="93584-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="93584-464">Por exemplo, se o retorno de chamada está fazendo uma WebRequest em relação a um recurso externo, esse código deve ser colocado no RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="93584-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="93584-465">**GetCallbackEvent** é responsável por processar o retorno da chamada de retorno ao cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="93584-466">Ele não utiliza argumentos e retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="93584-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="93584-467">Retorna a cadeia de caracteres será passada como um argumento para a função do lado do cliente, nesse caso *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="93584-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="93584-468">Depois de concluir as etapas acima, você está pronto para executar um retorno de chamada de script no ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="93584-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="93584-469">Abra vídeo de tela inteira</span><span class="sxs-lookup"><span data-stu-id="93584-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="93584-470">Retornos de chamada de script no ASP.NET têm suporte em qualquer navegador que dá suporte a chamadas de XMLHttp fazer.</span><span class="sxs-lookup"><span data-stu-id="93584-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="93584-471">Que inclui todos os navegadores modernos em uso atualmente.</span><span class="sxs-lookup"><span data-stu-id="93584-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="93584-472">Internet Explorer usa o objeto XMLHttp ActiveX enquanto outros navegadores modernos (incluindo o IE 7 futuras) usam um objeto intrínseco do XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="93584-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="93584-473">Para determinar programaticamente se um navegador dá suporte a retornos de chamada, você pode usar o **Request.Browser.SupportCallback** propriedade.</span><span class="sxs-lookup"><span data-stu-id="93584-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="93584-474">Esta propriedade retornará **true** se o cliente solicitante dá suporte a retornos de chamada de script.</span><span class="sxs-lookup"><span data-stu-id="93584-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="93584-475">Trabalhando com Script de cliente no ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="93584-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="93584-476">Scripts de cliente no ASP.NET 2.0 são gerenciados com o uso da classe ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="93584-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="93584-477">A classe ClientScriptManager controla de scripts de cliente usando um tipo e um nome.</span><span class="sxs-lookup"><span data-stu-id="93584-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="93584-478">Isso impede que o mesmo script programaticamente inseridos em uma página mais de uma vez.</span><span class="sxs-lookup"><span data-stu-id="93584-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="93584-479">Depois que um script tiver sido registrado com êxito em uma página, qualquer tentativa subsequente para registrar o mesmo script simplesmente resultará no script não está sendo registrado uma segunda vez.</span><span class="sxs-lookup"><span data-stu-id="93584-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="93584-480">Nenhum script duplicado é adicionada e nenhuma exceção ocorrer.</span><span class="sxs-lookup"><span data-stu-id="93584-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="93584-481">Para evitar o desnecessária computação, há métodos que você pode usar para determinar se um script já está registrado para que você não tente registrá-lo mais de uma vez.</span><span class="sxs-lookup"><span data-stu-id="93584-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="93584-482">Os métodos de ClientScriptManager devem estar familiarizados com todos os desenvolvedores ASP.NET atuais:</span><span class="sxs-lookup"><span data-stu-id="93584-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="93584-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="93584-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="93584-484">Este método adiciona um script para a parte superior da página renderizada.</span><span class="sxs-lookup"><span data-stu-id="93584-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="93584-485">Isso é útil para a adição de funções que serão chamadas explicitamente no cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="93584-486">Há duas versões sobrecarregadas desse método.</span><span class="sxs-lookup"><span data-stu-id="93584-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="93584-487">Três dos quatro argumentos são comuns entre eles.</span><span class="sxs-lookup"><span data-stu-id="93584-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="93584-488">Elas são:</span><span class="sxs-lookup"><span data-stu-id="93584-488">They are:</span></span>

`type (string)`

<span data-ttu-id="93584-489">O ***tipo*** argumento identifica um tipo para o script.</span><span class="sxs-lookup"><span data-stu-id="93584-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="93584-490">Geralmente é uma boa ideia usar tipo a página do (isso. GetType()) para o tipo.</span><span class="sxs-lookup"><span data-stu-id="93584-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="93584-491">O ***chave*** argumento é uma chave definida pelo usuário para o script.</span><span class="sxs-lookup"><span data-stu-id="93584-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="93584-492">Isso deve ser exclusivo para cada script.</span><span class="sxs-lookup"><span data-stu-id="93584-492">This should be unique for each script.</span></span> <span data-ttu-id="93584-493">Se você tentar adicionar um script com a mesma chave e tipo de um script já adicionado, ele não será adicionado.</span><span class="sxs-lookup"><span data-stu-id="93584-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="93584-494">O ***script*** argumento é uma cadeia de caracteres que contém o script real para adicionar.</span><span class="sxs-lookup"><span data-stu-id="93584-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="93584-495">É recomendável que você use StringBuilder para criar o script e, em seguida, use o método ToString () na StringBuilder para atribuir o ***script*** argumento.</span><span class="sxs-lookup"><span data-stu-id="93584-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="93584-496">Se você usar o RegisterClientScriptBlock sobrecarregado que só usa três argumentos, você deve incluir elementos de script (&lt;script&gt; e &lt;/script&gt;) em seu script.</span><span class="sxs-lookup"><span data-stu-id="93584-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="93584-497">Você pode optar por usar a sobrecarga do RegisterClientScriptBlock que usa um argumento quarto.</span><span class="sxs-lookup"><span data-stu-id="93584-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="93584-498">O quarto argumento é um valor booleano que especifica se o ASP.NET deve adicionar elementos de script para você.</span><span class="sxs-lookup"><span data-stu-id="93584-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="93584-499">Se esse argumento for **true**, o script não deve incluir os elementos de script explicitamente.</span><span class="sxs-lookup"><span data-stu-id="93584-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="93584-500">Use o método IsClientScriptBlockRegistered para determinar se um script já foi registrado.</span><span class="sxs-lookup"><span data-stu-id="93584-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="93584-501">Isso permite que você evite tentar registrar novamente um script que já foi registrado.</span><span class="sxs-lookup"><span data-stu-id="93584-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="93584-502">RegisterClientScriptInclude (novo no 2.0)</span><span class="sxs-lookup"><span data-stu-id="93584-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="93584-503">A marca RegisterClientScriptInclude cria um bloco de script que vincula a um arquivo de script externo.</span><span class="sxs-lookup"><span data-stu-id="93584-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="93584-504">Ele tem duas sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="93584-504">It has two overloads.</span></span> <span data-ttu-id="93584-505">Um usa uma chave e uma URL.</span><span class="sxs-lookup"><span data-stu-id="93584-505">One takes a key and a URL.</span></span> <span data-ttu-id="93584-506">A segunda adiciona um terceiro argumento de especificação do tipo.</span><span class="sxs-lookup"><span data-stu-id="93584-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="93584-507">Por exemplo, o código a seguir gera um bloco de script que vincula a jsfunctions.js na raiz da pasta de scripts do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="93584-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="93584-508">Esse código gera o código a seguir na página renderizada:</span><span class="sxs-lookup"><span data-stu-id="93584-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="93584-509">O bloco de script é renderizado na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="93584-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="93584-510">Use o método IsClientScriptIncludeRegistered para determinar se um script já foi registrado.</span><span class="sxs-lookup"><span data-stu-id="93584-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="93584-511">Isso permite que você evite tentar registrar novamente um script.</span><span class="sxs-lookup"><span data-stu-id="93584-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="93584-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="93584-512">RegisterStartupScript</span></span>

<span data-ttu-id="93584-513">O método de RegisterStartupScript usa os mesmos argumentos que o método RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="93584-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="93584-514">Um script registrado com RegisterStartupScript executa depois que a página for carregada, mas antes do evento OnLoad do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="93584-515">1. x, scripts registrados com RegisterStartupScript foram colocados antes do fechamento &lt;/formulário&gt; enquanto scripts registrados com RegisterClientScriptBlock foram feitos imediatamente após a abertura da tag &lt;formulário&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="93584-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="93584-516">No ASP.NET 2.0, ambos são colocadas imediatamente antes do fechamento &lt;/formulário&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="93584-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="93584-517">Se você registrar uma função com RegisterStartupScript, essa função não será executado até que você chamá-lo explicitamente no código do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="93584-518">Use o método IsStartupScriptRegistered para determinar se um script já foi registrado e evitar uma tentativa de registrar novamente um script.</span><span class="sxs-lookup"><span data-stu-id="93584-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="93584-519">Outros métodos de ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="93584-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="93584-520">Aqui estão alguns dos outros métodos úteis da classe ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="93584-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="93584-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="93584-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="93584-522">Consulte os retornos de chamada de script neste módulo.</span><span class="sxs-lookup"><span data-stu-id="93584-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="93584-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="93584-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="93584-524">Obtém uma referência de JavaScript (javascript:&lt;chamar&gt;) que pode ser usado para lançar novamente de um evento no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="93584-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="93584-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="93584-526">Obtém uma cadeia de caracteres que pode ser usada para iniciar uma postagem de volta do cliente.</span><span class="sxs-lookup"><span data-stu-id="93584-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="93584-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="93584-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="93584-528">Retorna uma URL para um recurso que é inserido em um assembly.</span><span class="sxs-lookup"><span data-stu-id="93584-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="93584-529">Deve ser usado em conjunto com <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="93584-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="93584-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="93584-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="93584-531">Registra um recurso da Web com a página.</span><span class="sxs-lookup"><span data-stu-id="93584-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="93584-532">Estes são os recursos incorporados em um assembly e manipuladas pelo novo manipulador WebResource.</span><span class="sxs-lookup"><span data-stu-id="93584-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="93584-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="93584-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="93584-534">Registra um campo de formulário oculto com a página.</span><span class="sxs-lookup"><span data-stu-id="93584-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="93584-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="93584-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="93584-536">Registra o código do lado do cliente que é executado quando o formulário HTML é enviado.</span><span class="sxs-lookup"><span data-stu-id="93584-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

