---
uid: web-pages/readme/beta3
title: Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor) | Microsoft Docs
author: rick-anderson
description: Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5aa609bf31499485dc7a1298fa689f3a7cee4774
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363527"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="1c298-103">Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1c298-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="1c298-104">Web Matrix e Leiame de versão Beta 3 do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="1c298-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="1c298-105">9 de novembro de 2010</span><span class="sxs-lookup"><span data-stu-id="1c298-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="1c298-106">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="1c298-106">Contents</span></span>

- [<span data-ttu-id="1c298-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="1c298-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="1c298-108">Instalação</span><span class="sxs-lookup"><span data-stu-id="1c298-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="1c298-109">Novos recursos, alterações e problemas conhecidos na versão Beta 3</span><span class="sxs-lookup"><span data-stu-id="1c298-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="1c298-110">Problemas de instalação do WebMatrix</span><span class="sxs-lookup"><span data-stu-id="1c298-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="1c298-111">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c298-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="1c298-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="1c298-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="1c298-113">Instalação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="1c298-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="1c298-114">Publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="1c298-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="1c298-115">Outros problemas</span><span class="sxs-lookup"><span data-stu-id="1c298-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="1c298-116">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="1c298-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="1c298-117">Visão geral</span><span class="sxs-lookup"><span data-stu-id="1c298-117">Overview</span></span>

> <span data-ttu-id="1c298-118">Microsoft WebMatrix Beta é uma pilha de desenvolvimento da web gratuito que instala em minutos.</span><span class="sxs-lookup"><span data-stu-id="1c298-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="1c298-119">Ele integra um servidor web com o banco de dados e estruturas para criar uma experiência única e integrada de programação.</span><span class="sxs-lookup"><span data-stu-id="1c298-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="1c298-120">Você pode usar o WebMatrix Beta para simplificar a maneira de código, testar e publicar seu próprio site ASP.NET ou PHP, ou você pode usar a versão Beta do WebMatrix para iniciar um novo site usando aplicativos de software livre populares, como o Joomla, Umbraco, WordPress ou DotNetNuke.</span><span class="sxs-lookup"><span data-stu-id="1c298-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="1c298-121">O WebMatrix Beta usa o mesmo servidor da web avançados, o mecanismo de banco de dados e o ambiente de estruturas que executará o seu site na internet, o que faz a transição do desenvolvimento para produção simples e direta.</span><span class="sxs-lookup"><span data-stu-id="1c298-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="1c298-122">Instalação</span><span class="sxs-lookup"><span data-stu-id="1c298-122">Installation</span></span>

> <span data-ttu-id="1c298-123">Para instalar o WebMatrix Beta 3, você deve usar [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="1c298-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="1c298-124">Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix Beta 3.</span><span class="sxs-lookup"><span data-stu-id="1c298-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="1c298-125">Se você tiver problemas durante a instalação, consulte [Solucionando problemas com o Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="1c298-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="1c298-126">Instruções para a publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="1c298-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="1c298-127">Consulte [instruções passo a passo para a publicação de aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="1c298-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="1c298-128">Novos recursos, alterações, andKnown problemas</span><span class="sxs-lookup"><span data-stu-id="1c298-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="1c298-129">Instalação do WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="1c298-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="1c298-130">Problema: O WebMatrix Beta 3 está disponível apenas em plataformas que dão suporte ao Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="1c298-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="1c298-131">O .NET Framework versão 4 é necessário para o WebMatrix Beta.</span><span class="sxs-lookup"><span data-stu-id="1c298-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="1c298-132">Em alguns casos, o instalador do WebMatrix Beta serão permitem que você tente instalar em uma plataforma que não faz parte do conjunto de configurações com suporte.</span><span class="sxs-lookup"><span data-stu-id="1c298-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="1c298-133">Em particular, Windows Vista sem a atualização do SP1 lhe permitirá começar a instalação da versão Beta do WebMatrix, mas o componente do .NET Framework 4 falhará e bloquear a instalação.</span><span class="sxs-lookup"><span data-stu-id="1c298-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="1c298-134">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-134">**Workaround**</span></span>  
> <span data-ttu-id="1c298-135">Instale em uma plataforma com suporte, que inclui:</span><span class="sxs-lookup"><span data-stu-id="1c298-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="1c298-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="1c298-136">Windows 7</span></span>
> - <span data-ttu-id="1c298-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="1c298-137">Windows Server 2008</span></span>
> - <span data-ttu-id="1c298-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="1c298-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="1c298-139">Windows Vista SP1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="1c298-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="1c298-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="1c298-140">Windows XP SP3</span></span>
> - <span data-ttu-id="1c298-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="1c298-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="1c298-142">Problema: Não é possível instalar o WebMatrix Beta 3, se o Microsoft Visual Studio 2008 é instalado sem o Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="1c298-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="1c298-143">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-143">**Workaround**</span></span>  
> <span data-ttu-id="1c298-144">Instale [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) do Centro de Download da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c298-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="1c298-145">Problema: Alguns assemblies para o SQL Server Compact 4.0 não estão instalados no GAC</span><span class="sxs-lookup"><span data-stu-id="1c298-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="1c298-146">Os assemblies gerenciados para o SQL Server Compact 4.0 não são colocados no cache de assembly global (GAC), quando você instala o SQL Server Compact 4.0 em um computador de 64 bits e o computador tem apenas o .NET Framework 3.5 SP1 Client Profile instalado.</span><span class="sxs-lookup"><span data-stu-id="1c298-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="1c298-147">São os assemblies gerenciados que não estão instalados no GAC:</span><span class="sxs-lookup"><span data-stu-id="1c298-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="1c298-148">*SqlServerCe* (provedor de ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="1c298-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="1c298-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="1c298-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="1c298-150">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-150">**Workaround**</span></span>  
> <span data-ttu-id="1c298-151">Desinstalar o SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="1c298-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="1c298-152">Baixe e instale a versão completa do .NET Framework 3.5 SP1 no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="1c298-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="1c298-153">Microsoft .NET Framework 3.5 Service pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="1c298-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="1c298-154">Em seguida, reinstale o SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="1c298-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="1c298-155">Problema: Não é possível desinstalar o SQL Server Compact usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="1c298-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="1c298-156">Desinstalação do SQL Server Compact usando as opções de linha de comando não funciona nesta versão.</span><span class="sxs-lookup"><span data-stu-id="1c298-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="1c298-157">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-157">**Workaround**</span></span>  
> <span data-ttu-id="1c298-158">Use *programas e recursos* no painel de controle do Windows para desinstalar o Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="1c298-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="1c298-159">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c298-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="1c298-160">Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a versão Beta 3 do ASP.NET Web Pages com sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="1c298-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="1c298-161">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="1c298-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="1c298-162">Alterações</span><span class="sxs-lookup"><span data-stu-id="1c298-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="1c298-163">Problemas</span><span class="sxs-lookup"><span data-stu-id="1c298-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="1c298-164">Novos recursos na versão Beta 3 páginas da Web ASP.NET com sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="1c298-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="1c298-165">Novo: O método de "HTML. Raw" renderiza marcação sem codificação</span><span class="sxs-lookup"><span data-stu-id="1c298-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="1c298-166">O novo `Html.Raw` método permite renderizar marcação HTML como marcação em vez de renderizar a saída codificada.</span><span class="sxs-lookup"><span data-stu-id="1c298-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="1c298-167">(Por padrão, ASP.NET Razor codifica cadeias de caracteres antes de renderizá-los.) A sintaxe é:</span><span class="sxs-lookup"><span data-stu-id="1c298-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="1c298-168">O exemplo a seguir mostra como usar `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="1c298-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="1c298-169">Alterações na versão Beta 3 páginas da Web ASP.NET com sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="1c298-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="1c298-170">Alterar: "HrefAttribute" método removido</span><span class="sxs-lookup"><span data-stu-id="1c298-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="1c298-171">O `HrefAttribute` método da `WebPage` classe foi removida.</span><span class="sxs-lookup"><span data-stu-id="1c298-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="1c298-172">Este auxiliar foi usado para codificar caracteres não seguros em URLs.</span><span class="sxs-lookup"><span data-stu-id="1c298-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="1c298-173">Não é necessário porque o ASP.NET Razor automaticamente codifica cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1c298-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="1c298-174">(Use o novo `Html.Raw` método para renderizar cadeias de caracteres sem codificação.)</span><span class="sxs-lookup"><span data-stu-id="1c298-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="1c298-175">Alteração: Sintaxe para declarativo "@helper" auxiliares alterados</span><span class="sxs-lookup"><span data-stu-id="1c298-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="1c298-176">Na versão Beta 3, ASP.NET muda como ele analisa os auxiliares que são criados usando o `@helper` sintaxe.</span><span class="sxs-lookup"><span data-stu-id="1c298-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="1c298-177">Em essência, o `@helper` sintaxe agora é analisado como um bloco de código em vez de como um bloco de marcação que pode incluir código.</span><span class="sxs-lookup"><span data-stu-id="1c298-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="1c298-178">Portanto, o código dentro do auxiliar não precisa ser colocado entre `@{ }` blocos.</span><span class="sxs-lookup"><span data-stu-id="1c298-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="1c298-179">Por outro lado, marcação dentro do auxiliar deve ser incluído explicitamente em elementos HTML ou em ASP.NET Razor `<text></text>` marcas.</span><span class="sxs-lookup"><span data-stu-id="1c298-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="1c298-180">Por exemplo, a seguinte `@helper` sintaxe funciona na versão Beta 3:</span><span class="sxs-lookup"><span data-stu-id="1c298-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="1c298-181">Na versão Beta 3, esse auxiliar deve ser alterada para se parecer com o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="1c298-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="1c298-182">Observe que o `@{ }` caracteres ao redor do código inicial no auxiliar não é mais usada.</span><span class="sxs-lookup"><span data-stu-id="1c298-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="1c298-183">Isso ocorre porque o conteúdo dos auxiliares é tratado como um bloco de código por padrão.</span><span class="sxs-lookup"><span data-stu-id="1c298-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="1c298-184">O auxiliar de marcação, que começa com a abertura de renderiza `<a>` marca.</span><span class="sxs-lookup"><span data-stu-id="1c298-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="1c298-185">Se o auxiliar deve renderizar texto sem formatação ou marcas que não incluem uma marca de fechamento (por exemplo, `<meta>` marcas), o conteúdo a ser renderizado deve estar no `<text></text>` marcas.</span><span class="sxs-lookup"><span data-stu-id="1c298-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="1c298-186">Alterações: "WebPageContext.HttpContext" removidas</span><span class="sxs-lookup"><span data-stu-id="1c298-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="1c298-187">O `WebPageContext.HttpContext` propriedade foi removida.</span><span class="sxs-lookup"><span data-stu-id="1c298-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="1c298-188">Use `HttpContext.Current` em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="1c298-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="1c298-189">(O `WebPageContext.HttpContext` propriedade simplesmente encapsulada isso.)</span><span class="sxs-lookup"><span data-stu-id="1c298-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="1c298-190">Alteração: Auxiliar de "Facebook" movido para o novo pacote</span><span class="sxs-lookup"><span data-stu-id="1c298-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="1c298-191">O `Facebook` auxiliar foi movido para o *Facebook.Helper* biblioteca, que inclui o `Facebook` auxiliar e funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="1c298-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="1c298-192">Você deve instalar essa biblioteca como um pacote separado, conforme descrito em "Instalar auxiliares com o Gerenciador de pacotes" no tutorial [Introdução a páginas ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="1c298-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="1c298-193">Alteração: Tipos de associação, a função e a segurança move para o novo assembly</span><span class="sxs-lookup"><span data-stu-id="1c298-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="1c298-194">Os seguintes tipos foram movidos para o `WebMatrix.WebData` assembly:</span><span class="sxs-lookup"><span data-stu-id="1c298-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="1c298-195">Alteração: Classe de "TagBuilder" movido para o assembly System.Web.WebPages.dll</span><span class="sxs-lookup"><span data-stu-id="1c298-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="1c298-196">O `TagBuilde` classe r foi movido para o assembly System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="1c298-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="1c298-197">Anteriormente, isso era em um assembly que fazia parte do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1c298-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="1c298-198">Essa alteração significa que você não precisa instalar o ASP.NET MVC para usar o `TagBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="1c298-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="1c298-199">No entanto, a classe ainda está no `System.Web.Mvc` namespace.</span><span class="sxs-lookup"><span data-stu-id="1c298-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="1c298-200">Para usar o `TagBuilder` classe (por exemplo, em um ASP.NET Razor auxiliar personalizado), você deve fazer referência ao namespace (por exemplo, adicionando `@using System.Web.Mvc` ao seu código).</span><span class="sxs-lookup"><span data-stu-id="1c298-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="1c298-201">Alteração: Alterado; de sintaxe de validação de solicitação Classe de "Validação" removido</span><span class="sxs-lookup"><span data-stu-id="1c298-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="1c298-202">Na versão Beta 3, para desabilitar a validação para um campo individual ou um conjunto de campos, você pode chamar o `Validation.Exclude` método, passando o nome ou os nomes dos campos a serem excluídos da validação.</span><span class="sxs-lookup"><span data-stu-id="1c298-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="1c298-203">Uma nova sintaxe está disponível na versão Beta 3 para ignorar a validação.</span><span class="sxs-lookup"><span data-stu-id="1c298-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="1c298-204">O `Validation` método usado na versão Beta 3 foi removido.</span><span class="sxs-lookup"><span data-stu-id="1c298-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="1c298-205">Se você não desabilitar a validação de solicitação, se os usuários tentarem carregar uma marcação HTML (por exemplo, usando um editor de rich text em uma página), o site relatará um erro como *um valor Request. Form potencialmente perigoso foi detectado no cliente*e a entrada do usuário não é aceito.</span><span class="sxs-lookup"><span data-stu-id="1c298-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="1c298-206">Se você desabilitar a validação de solicitação, você deve verificar manualmente a entrada do usuário para certificar-se de que ele não contém marcação potencialmente perigosa ou de script usando algo como o [Microsoft anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="1c298-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="1c298-207">Para desabilitar a validação de solicitação automática, chame o `Request.Unvalidated` método, passando o nome do campo ou outro objeto de postagem que deseja ignorar a validação de solicitação para.</span><span class="sxs-lookup"><span data-stu-id="1c298-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="1c298-208">Você pode usar esse método para ignorar a validação para todos os itens na `Form`, `QueryString`, `Cookies`, e `ServerVariables` coleções.</span><span class="sxs-lookup"><span data-stu-id="1c298-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="1c298-209">Os exemplos a seguir mostram como usar o `Unvalidated` método:</span><span class="sxs-lookup"><span data-stu-id="1c298-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="1c298-210">Problemas conhecidos do ASP.NET Web Pages com sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="1c298-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="1c298-211">Problema: Um comportamento inesperado ao usar uma tabela de usuário personalizada para a associação</span><span class="sxs-lookup"><span data-stu-id="1c298-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="1c298-212">Para inicializar o provedor de associação para um site do ASP.NET Razor, você deve chamar o `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="1c298-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="1c298-213">(No WebMatrix, o modelo Site inicial inclui uma chamada para esse método na  *\_AppStart.cshtml* arquivo.) Se o `autoCreateTables` parâmetro desse método é definido como true (por padrão, ele é definido como true no modelo de Site inicial), e se um nome de tabela não reconhecido é passado para o método (o segundo parâmetro), o método não lança um erro.</span><span class="sxs-lookup"><span data-stu-id="1c298-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="1c298-214">Em vez disso, ele automaticamente cria a tabela.</span><span class="sxs-lookup"><span data-stu-id="1c298-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="1c298-215">Isso pode ser um problema se você pretende usar uma tabela de usuário personalizada para a associação, mas passe o nome da tabela incorreta para o `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="1c298-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="1c298-216">Como o método por padrão gerará um erro se a tabela que você especificar não existir, e em vez disso, ele cria uma nova tabela, o aplicativo pode parecer estar funcionando.</span><span class="sxs-lookup"><span data-stu-id="1c298-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="1c298-217">No entanto, o código do aplicativo que se baseia em sua tabela de usuário personalizada (e em campos nele), eventualmente, pode falhar com erros inesperados.</span><span class="sxs-lookup"><span data-stu-id="1c298-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="1c298-218">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-218">**Workaround**</span></span>  
> <span data-ttu-id="1c298-219">Certifique-se de que o nome passado a `InitializeDatabaseConnection` correspondências de método, o perfil do usuário no banco de dados de associação de tabela, ou certifique-se de que o `autoCreateTables` parâmetro é definido como false.</span><span class="sxs-lookup"><span data-stu-id="1c298-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="1c298-220">Erro de problema: "Falha ao gerar uma instância de usuário do SQL Server"</span><span class="sxs-lookup"><span data-stu-id="1c298-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="1c298-221">Se um aplicativo Web do WebMatrix usa o SQL Server Express e está executando o IIS 7.5 no Windows 7 ou Windows Server 2008 R2, você poderá ver um erro que indica que o SQL Server não pode recuperar caminho do aplicativo local do usuário em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1c298-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="1c298-222">**Solução alternativa** Certifique-se de que a conta do Windows que o aplicativo é executado (normalmente o serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo em subpastas, como *aplicativo\_dados*.</span><span class="sxs-lookup"><span data-stu-id="1c298-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="1c298-223">Informações mais detalhadas estão disponíveis no artigo da Base de Conhecimento [problemas com a instância de usuário do SQL Server Express e o ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="1c298-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="1c298-224">Problema: No Visual Studio, namespaces para assemblies personalizados (DLLs) não são importados automaticamente</span><span class="sxs-lookup"><span data-stu-id="1c298-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="1c298-225">Se você usar assemblies personalizados em um projeto no Visual Studio, os namespaces declarados nesses assemblies não são importados automaticamente no tempo de design.</span><span class="sxs-lookup"><span data-stu-id="1c298-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="1c298-226">Como resultado, as referências a tipos personalizados não podem ser reconhecidas em tempo de design e são marcadas como não reconhecido no Visual Studio (usando uma "linha ondulada").</span><span class="sxs-lookup"><span data-stu-id="1c298-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="1c298-227">Esse problema ocorre somente em tempo de design no Visual Studio; o aplicativo em si seja executado corretamente.</span><span class="sxs-lookup"><span data-stu-id="1c298-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="1c298-228">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-228">**Workaround**</span></span>  
> <span data-ttu-id="1c298-229">Incluir um `using` instrução (`imports` no Visual Basic) que faz referência as entidades que não são reconhecidas em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="1c298-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="1c298-230">Problema: Visual Studio IntelliSense e o projeto de modelos disponíveis apenas no ASP.NET MVC versão 3</span><span class="sxs-lookup"><span data-stu-id="1c298-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="1c298-231">Instalar páginas da Web ASP.NET não também instalará as ferramentas para Visual Studio como IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1c298-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="1c298-232">**Solução alternativa** para usar o IntelliSense e modelos de projeto para aplicativos de páginas da Web ASP.NET no Visual Studio, instalar o ASP.NET MVC 3 RC por meio do Web Platform Installer ou o [instalador autônomo](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="1c298-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="1c298-233">Problema: "&lt;auxiliar&gt; classe não pode ser encontrado" Erro</span><span class="sxs-lookup"><span data-stu-id="1c298-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="1c298-234">Depois de atualizar a versão Beta 3, você poderá ver um erro que uma classe auxiliar (por exemplo, o `Facebook` classe) não pode ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="1c298-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="1c298-235">Começando na versão Beta 2 e continuando na versão Beta 3, os auxiliares foram movidos para pacotes que você deve instalar explicitamente.</span><span class="sxs-lookup"><span data-stu-id="1c298-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="1c298-236">Sites existentes não são atualizados para incluir esses pacotes; Isso inclui sites na *\My Documents\IISExpress* ou *\My Sites da Web* pastas.</span><span class="sxs-lookup"><span data-stu-id="1c298-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="1c298-237">Em particular, você verá esse erro se você usar o site padrão no *Meus Sites* (WebSite1), que inclui uma referência para o `Twitter` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="1c298-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="1c298-238">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-238">**Workaround**</span></span>  
> <span data-ttu-id="1c298-239">Comente a chamadas para qualquer auxiliares no site, execute as  *\_Admin* da página e instalar o pacote ou pacotes que incluem os auxiliares que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="1c298-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="1c298-240">Depois de instalar o pacote, você pode remover o comentário as linhas que fazem referência a auxiliares.</span><span class="sxs-lookup"><span data-stu-id="1c298-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="1c298-241">Problema: Implantar assemblies do Beta 3 ASP.NET Razor para a pasta Bin pode não funcionar na hospedagem de sites</span><span class="sxs-lookup"><span data-stu-id="1c298-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="1c298-242">Se você implantar um site de páginas da Web ASP.NET em um site de hospedagem, e se você implantar os assemblies do ASP.NET Razor Beta 3 para o site *Bin* pasta, você pode enfrentar erros, incluindo o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="1c298-243">Isso pode acontecer se o provedor de hospedagem tiver instalado os assemblies do ASP.NET Web Pages Beta 1 no cache do servidor de aplicativo global (GAC).</span><span class="sxs-lookup"><span data-stu-id="1c298-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="1c298-244">Assemblies no GAC têm precedência sobre os assemblies instalados localmente na *Bin* pasta.</span><span class="sxs-lookup"><span data-stu-id="1c298-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="1c298-245">**Solução alternativa** entre em contato com seu provedor de hospedagem para confirmar se os erros que você está vendo devido a um conflito entre versões do provedor de assemblies e o seu.</span><span class="sxs-lookup"><span data-stu-id="1c298-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="1c298-246">Nesse caso, solicite que o provedor de hospedagem atualizem os assemblies no GAC do servidor.</span><span class="sxs-lookup"><span data-stu-id="1c298-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="1c298-247">Problema: Leitura de feeds ou outros dados externos por meio de um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="1c298-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="1c298-248">Se o servidor que executa o site estiver atrás de um servidor proxy, você talvez precise configurar informações de proxy nas *Web. config* arquivo para que seja possível ler informações provenientes de fora do site.</span><span class="sxs-lookup"><span data-stu-id="1c298-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="1c298-249">Por exemplo, se você usar o `ReCaptcha` auxiliar, o auxiliar se comunica com o serviço reCAPTCHA, mas podem ser bloqueado pelo seu servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="1c298-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="1c298-250">Problema: Usando o projeto de aplicativo Web ou páginas ASP.NET MVC e Web ASP.NET no mesmo aplicativo</span><span class="sxs-lookup"><span data-stu-id="1c298-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="1c298-251">Se você estivesse usando páginas da Web ASP.NET em um projeto de aplicativo Web ou um aplicativo ASP.NET MVC, você poderá ver um erro que *WebPageHttpApplication* não pode ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="1c298-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="1c298-252">Se você receber esse erro, altere a classe base da qual deriva o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="1c298-253">No global. asax file, altere a linha a seguir:</span><span class="sxs-lookup"><span data-stu-id="1c298-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="1c298-254">Para isso:</span><span class="sxs-lookup"><span data-stu-id="1c298-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="1c298-255">Isso inverte a uma alteração que foi introduzida em vigor para a versão Beta 1 do ASP.NET Web Pages com sintaxe do Razor.</span><span class="sxs-lookup"><span data-stu-id="1c298-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="1c298-256">Problema: Implantando um aplicativo em um computador que não tenha o SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="1c298-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="1c298-257">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-257">**Workaround**</span></span>  
> <span data-ttu-id="1c298-258">Aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador em que o SQL Server Compact não está instalado.</span><span class="sxs-lookup"><span data-stu-id="1c298-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="1c298-259">Microsoft WebMatrix Beta 3 copia esses binários para você automaticamente e executa apropriado Web. config transformações de arquivo.</span><span class="sxs-lookup"><span data-stu-id="1c298-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="1c298-260">Solução alternativa se você precisar copiar esses arquivos e fazer a Web. config alterações de arquivo manualmente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="1c298-261">Copie os assemblies do mecanismo de banco de dados para o Bin pasta (e as subpastas) do aplicativo no computador de destino:</span><span class="sxs-lookup"><span data-stu-id="1c298-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="1c298-262">Cópia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86*para \Bin\x86</span><span class="sxs-lookup"><span data-stu-id="1c298-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="1c298-263">Cópia *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64*para \Bin\amd64</span><span class="sxs-lookup"><span data-stu-id="1c298-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="1c298-264">Na pasta raiz do site, crie ou abra uma Web. config arquivo.</span><span class="sxs-lookup"><span data-stu-id="1c298-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="1c298-265">**(No WebMatrix Beta 3, esse tipo de arquivo estará disponível se você clicar **todos os** na escolher um tipo de arquivo caixa de diálogo.)</span><span class="sxs-lookup"><span data-stu-id="1c298-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="1c298-266">Adicione o seguinte elemento como um filho do ** configuration  elemento (não dentro de  System. Web  elemento):</span><span class="sxs-lookup"><span data-stu-id="1c298-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="1c298-267">Problema: Os auxiliares de banco de dados e o WebGrid não funcionam no nível de confiança média no Visual Basic</span><span class="sxs-lookup"><span data-stu-id="1c298-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="1c298-268">Se você estiver usando o Visual Basic (criando *. vbhtml* arquivos), o *e* auxiliares não funcionará se o aplicativo está configurado para usar confiança média.</span><span class="sxs-lookup"><span data-stu-id="1c298-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="1c298-269">Defina temporariamente o aplicativo para usar a confiança total. Problema: A propriedade "Encrypt" não é reconhecida</span><span class="sxs-lookup"><span data-stu-id="1c298-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="1c298-270">SQL Server Compact 4.0 não reconhece o  propriedade do  classe.</span><span class="sxs-lookup"><span data-stu-id="1c298-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="1c298-271">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="1c298-271">**Workaround** </span></span>  
> <span data-ttu-id="1c298-272">Você não deve usar essa propriedade para criptografar arquivos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c298-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="1c298-273">O  propriedade foi preterida na versão 3.5 compacto do SQL Server e foi mantida somente para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="1c298-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="1c298-274">Use o *propriedade do* classe para criptografar arquivos de banco de dados do SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="1c298-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="1c298-275">O exemplo a seguir mostra como criar um banco de dados SQL Server Compact 4.0 criptografada usando o  propriedade:</span><span class="sxs-lookup"><span data-stu-id="1c298-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="1c298-276">Para alterar o modo de criptografia de banco de dados SQL Server Compact 4.0 existente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="1c298-277">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="1c298-278">Para criptografar um banco de dados do SQL Server Compact 4.0 não criptografado, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="1c298-279">Problema: Bibliotecas de tempo de execução do Microsoft Visual C++ 2008 são necessárias</span><span class="sxs-lookup"><span data-stu-id="1c298-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="1c298-280">O native DLLs do SQL Server Compact 4.0 é necessário o Microsoft Visual C++ 2008 bibliotecas de Runtime (x86, IA64 e x64), o Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="1c298-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="1c298-281">Instale o .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="1c298-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="1c298-282">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-282">**Workaround**</span></span>  
> <span data-ttu-id="1c298-283">Isso também instala o Visual C++ 2008 tempo de execução de bibliotecas do SP1.</span><span class="sxs-lookup"><span data-stu-id="1c298-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="1c298-284">Você pode baixar as bibliotecas do seguinte local:</span><span class="sxs-lookup"><span data-stu-id="1c298-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="1c298-285">Atualização de segurança ATL do Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package</span><span class="sxs-lookup"><span data-stu-id="1c298-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="1c298-286">Observe que o .NET Framework 2.0, 3.0, a instalação ou 4 faz não instalar o SP1 de bibliotecas de tempo de execução do Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="1c298-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="1c298-287">Problema: O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação</span><span class="sxs-lookup"><span data-stu-id="1c298-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="1c298-288">O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação de privilégio, como a instalação de componentes adicionais nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="1c298-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="1c298-289">No Windows Vista ou Windows 7, você está conectado com uma conta que não tem privilégios administrativos e controle de conta de usuário (UAC) está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="1c298-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="1c298-290">**Você está usando o Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="1c298-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="1c298-291">A maioria das tarefas no WebMatrix Beta 3 não exigem permissão administrativa.</span><span class="sxs-lookup"><span data-stu-id="1c298-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="1c298-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="1c298-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="1c298-293">Para aqueles que fazer, você pode executar a operação como um administrador ou siga estas etapas:\*</span><span class="sxs-lookup"><span data-stu-id="1c298-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="1c298-294">No Windows Vista ou Windows 7, habilite o UAC.\*</span><span class="sxs-lookup"><span data-stu-id="1c298-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="1c298-295">No Windows XP, adicione o usuário ao grupo de segurança Administradores.</span><span class="sxs-lookup"><span data-stu-id="1c298-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="1c298-296">Problema: "Site da Galeria de Web" está desabilitada</span><span class="sxs-lookup"><span data-stu-id="1c298-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="1c298-297">O **Site da Galeria de Web&lt; opção será desabilitada se o Web Platform Installer 3.0 não está instalado.</span><span class="sxs-lookup"><span data-stu-id="1c298-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="1c298-298">Instalar o Microsoft Web Platform Installer 3.0.</span><span class="sxs-lookup"><span data-stu-id="1c298-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="1c298-299">Problema: No Windows Server 2003, o IIS Express não é iniciado para um usuário não administrativo</span><span class="sxs-lookup"><span data-stu-id="1c298-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="1c298-300">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-300">**Workaround**</span></span>  
> <span data-ttu-id="1c298-301">No Windows Server 2003, quando você iniciar uma página ou iniciar o IIS Express, o IIS Express não é iniciado.</span><span class="sxs-lookup"><span data-stu-id="1c298-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="1c298-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="1c298-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="1c298-303">Páginas da Web, um erro será exibido que indica se o aplicativo foi iniciado por um usuário não administrativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="1c298-304">Inicie o WebMatrix Beta 3 como um usuário administrativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="1c298-305">Para obter mais detalhes, consulte o seguinte artigo da Base de Conhecimento:</span><span class="sxs-lookup"><span data-stu-id="1c298-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="1c298-306">Um aplicativo que é iniciado por um usuário não administrativo não pode ouvir o tráfego HTTP do computador no qual o aplicativo é executado no Windows Vista, Windows Server 2003 ou Windows XP.</span><span class="sxs-lookup"><span data-stu-id="1c298-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="1c298-307">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-307">**Workaround**</span></span>  
> <span data-ttu-id="1c298-308">Problema: O Google Chrome não está disponível como uma opção de execução</span><span class="sxs-lookup"><span data-stu-id="1c298-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="1c298-309">Google Chrome não é exibido na lista de navegadores em `Encryption Mode`executados sobre o Home guia.</span><span class="sxs-lookup"><span data-stu-id="1c298-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="1c298-310">Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows.</span><span class="sxs-lookup"><span data-stu-id="1c298-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="1c298-311">Como alternativa, inicie o Google Chrome, clique o personalizar e controle Google Chrome menu, clique em opçõese, em seguida, clique em Make Google Chrome meu navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="1c298-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="1c298-312">Problema: A caixa de diálogo "Foreign Key" não permite inserir uma chave primária</span><span class="sxs-lookup"><span data-stu-id="1c298-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="1c298-313">O Foreign Key caixa de diálogo não permite que você insira o nome da chave primário da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="1c298-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="1c298-314">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-314">**Workaround**</span></span>  
> <span data-ttu-id="1c298-315">Isso é intencional.</span><span class="sxs-lookup"><span data-stu-id="1c298-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="1c298-316">Você não precisará inserir o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="1c298-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="1c298-317">Problema: O botão "Relações" está desabilitado</span><span class="sxs-lookup"><span data-stu-id="1c298-317">You can download the libraries from the following location:</span></span>   
>   
> <span data-ttu-id="1c298-318">[O [relacionamentos](https://go.microsoft.com/fwlink/?LinkId=194827) botão sob o tabela guia o bancos de dados espaço de trabalho está desabilitado para bancos de dados do SQL Server Compact.](https://go.microsoft.com/fwlink/?LinkId=194827)</span><span class="sxs-lookup"><span data-stu-id="1c298-318">[Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update](https://go.microsoft.com/fwlink/?LinkId=194827)</span></span>
> 
> [!NOTE]
> <span data-ttu-id="1c298-319">O SQL Server Compact não oferece suporte a relações entre tabelas.</span><span class="sxs-lookup"><span data-stu-id="1c298-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="1c298-320">Problema: Consultas SQL parametrizadas lançam exceções</span><span class="sxs-lookup"><span data-stu-id="1c298-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="1c298-321">No SQL Server Compact 4.0, se você não especificar um tipo de dados, como  ou  para parâmetros em consultas parametrizadas, uma exceção é lançada quando a consulta é executada.</span><span class="sxs-lookup"><span data-stu-id="1c298-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="1c298-322">Definir explicitamente o tipo de dados para parâmetros, como *ou*.</span><span class="sxs-lookup"><span data-stu-id="1c298-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="1c298-323">Isso é importante no caso de tipos de dados BLOB (*e*).</span><span class="sxs-lookup"><span data-stu-id="1c298-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="1c298-324">Use o código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="1c298-325">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-325">**Workaround**</span></span>  
> <span data-ttu-id="1c298-326">Para obter mais informações sobre o WebMatrix Beta 3, consulte os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="1c298-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="1c298-327">Todos os direitos reservados.</span><span class="sxs-lookup"><span data-stu-id="1c298-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="1c298-328">Microsoft .NET Framework 3.5 Service pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="1c298-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> <span data-ttu-id="1c298-329">[Termos de uso](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en).](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)</span><span class="sxs-lookup"><span data-stu-id="1c298-329">[Microsoft .NET Framework 4.0 Release (Full Package)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)</span></span>
> 
> <span data-ttu-id="1c298-330">Em seguida, reinstalar [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="1c298-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="1c298-331">Instalação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="1c298-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="1c298-332">Problema: Instalação de um aplicativo pode levar muito tempo se a pasta Meus documentos do usuário é redirecionada para um compartilhamento de rede</span><span class="sxs-lookup"><span data-stu-id="1c298-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="1c298-333">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-333">**Workaround**</span></span>  
> <span data-ttu-id="1c298-334">nenhuma.</span><span class="sxs-lookup"><span data-stu-id="1c298-334">None.</span></span> <span data-ttu-id="1c298-335">O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="1c298-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="1c298-336">Publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="1c298-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="1c298-337">Problema: Os sites podem não funcionar após a publicação se o campo "URL de destino" não é prefixado com http:// ou https://</span><span class="sxs-lookup"><span data-stu-id="1c298-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="1c298-338">No **configurações de publicação** caixa de diálogo, se a URL de destino não começa com `http://` ou `https://`, o site pode não funcionar após a implantação.</span><span class="sxs-lookup"><span data-stu-id="1c298-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="1c298-339">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-339">**Workaround**</span></span>  
> <span data-ttu-id="1c298-340">Certifique-se de que antes de publicar um site, a URL de destino na **configurações de publicação** caixa de diálogo começa com `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="1c298-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="1c298-341">Problema: Um banco de dados MySQL de publicação falha com o erro "Falha ao publicar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c298-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="1c298-342">Isso pode acontecer se o banco de dados remoto não é possível executar o script."</span><span class="sxs-lookup"><span data-stu-id="1c298-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="1c298-343">O erro pode ocorrer por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="1c298-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="1c298-344">Um motivo que você pode ver esse erro é se o script de banco de dados contém um caractere de aspas simples (') e o conjunto de caracteres de padrão de destino MySQL do banco de dados não está em UTF-8.</span><span class="sxs-lookup"><span data-stu-id="1c298-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="1c298-345">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-345">**Workaround**</span></span>  
> <span data-ttu-id="1c298-346">Defina o caractere padrão definido para o banco de dados remoto do MySQL como UTF-8.</span><span class="sxs-lookup"><span data-stu-id="1c298-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="1c298-347">Outros Problemas</span><span class="sxs-lookup"><span data-stu-id="1c298-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="1c298-348">Problema: / Filtro de pesquisa não funciona em relatórios para agrupar por: tipo de problema</span><span class="sxs-lookup"><span data-stu-id="1c298-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="1c298-349">Quando você executa um relatório para um site, se você inserir texto na *filtrar por URL* caixa e clique em *pesquisa*, nada acontece.</span><span class="sxs-lookup"><span data-stu-id="1c298-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="1c298-350">Isso ocorre porque esse controle não está funcionando enquanto os *Group By* o estado do relatório é definido como *tipo de problema*, que é o padrão.</span><span class="sxs-lookup"><span data-stu-id="1c298-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="1c298-351">**Solução alternativa** no *Group By* guia da faixa de opções, clique em *URL* para agrupar as entradas por sua URL de origem.</span><span class="sxs-lookup"><span data-stu-id="1c298-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="1c298-352">A caixa de texto e o botão para filtrar as entradas são funcionais enquanto nesse estado.</span><span class="sxs-lookup"><span data-stu-id="1c298-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="1c298-353">Problema: Os aplicativos do WCF não funcionará com o IIS Express</span><span class="sxs-lookup"><span data-stu-id="1c298-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="1c298-354">Navegando para um aplicativo WCF resulta em um erro semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="1c298-355">*Não foi possível carregar arquivo ou assembly ' Administration, versão = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou uma de suas dependências. O sistema não pode localizar o arquivo especificado.*</span><span class="sxs-lookup"><span data-stu-id="1c298-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="1c298-356">Isso ocorre porque a versão do IIS Express Beta não dá suporte a WCF por padrão.</span><span class="sxs-lookup"><span data-stu-id="1c298-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="1c298-357">**Solução alternativa** usar qualquer uma das seguintes alternativas (solução alternativa 2 # requer o Microsoft Windows Vista ou superior):</span><span class="sxs-lookup"><span data-stu-id="1c298-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="1c298-358">Cópia de *Microsoft.Web.dll* e *Administration* assemblies do local de instalação do WebMatrix para o *bin* do WCF aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="1c298-359">Por padrão, o WebMatrix é instalado na *o Microsoft WebMatrix* subpasta do sistema *arquivos de programas* pasta.</span><span class="sxs-lookup"><span data-stu-id="1c298-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="1c298-360">No Microsoft Windows Vista ou versões posteriores, crie um symlink para os assemblies na *bin* usando os seguintes comandos do diretório.</span><span class="sxs-lookup"><span data-stu-id="1c298-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="1c298-361">(Essa abordagem tem a vantagem que ele não cria uma cópia dos assemblies.)</span><span class="sxs-lookup"><span data-stu-id="1c298-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="1c298-362">Instale os dois assemblies no GAC.</span><span class="sxs-lookup"><span data-stu-id="1c298-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="1c298-363">Em um prompt com privilégios elevados, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="1c298-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="1c298-364">Problema: O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação</span><span class="sxs-lookup"><span data-stu-id="1c298-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="1c298-365">O WebMatrix Beta 3 é capaz de realizar certas tarefas que exigem elevação de privilégio, como a instalação de componentes adicionais nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="1c298-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="1c298-366">No Windows Vista ou Windows 7, você está conectado com uma conta que não tem privilégios administrativos e controle de conta de usuário (UAC) está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="1c298-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="1c298-367">Você está usando o Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="1c298-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="1c298-368">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-368">**Workaround**</span></span>  
> <span data-ttu-id="1c298-369">A maioria das tarefas no WebMatrix Beta 3 não exigem permissão administrativa.</span><span class="sxs-lookup"><span data-stu-id="1c298-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="1c298-370">Para aqueles que fazer, você pode executar a operação como um administrador ou siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="1c298-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="1c298-371">No Windows Vista ou Windows 7, habilite o UAC.</span><span class="sxs-lookup"><span data-stu-id="1c298-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="1c298-372">No Windows XP, adicione o usuário ao grupo de segurança Administradores.</span><span class="sxs-lookup"><span data-stu-id="1c298-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="1c298-373">Problema: "Site da Galeria de Web" está desabilitada</span><span class="sxs-lookup"><span data-stu-id="1c298-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="1c298-374">O **Site da Galeria de Web** opção será desabilitada se o Web Platform Installer 3.0 não está instalado.</span><span class="sxs-lookup"><span data-stu-id="1c298-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="1c298-375">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-375">**Workaround**</span></span>  
> <span data-ttu-id="1c298-376">Instalar o [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="1c298-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="1c298-377">Problema: No Windows Server 2003, o IIS Express não é iniciado para um usuário não administrativo</span><span class="sxs-lookup"><span data-stu-id="1c298-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="1c298-378">No Windows Server 2003, quando você iniciar uma página ou iniciar o IIS Express, o IIS Express não é iniciado.</span><span class="sxs-lookup"><span data-stu-id="1c298-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="1c298-379">Páginas da Web, um erro será exibido que indica se o aplicativo foi iniciado por um usuário não administrativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="1c298-380">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-380">**Workaround**</span></span>  
> <span data-ttu-id="1c298-381">Inicie o WebMatrix Beta 3 como um usuário administrativo.</span><span class="sxs-lookup"><span data-stu-id="1c298-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="1c298-382">Para obter mais detalhes, consulte o seguinte artigo da Base de Conhecimento:</span><span class="sxs-lookup"><span data-stu-id="1c298-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="1c298-383">Um aplicativo que é iniciado por um usuário não administrativo não pode ouvir o tráfego HTTP do computador no qual o aplicativo é executado no Windows Vista, Windows Server 2003 ou Windows XP.</span><span class="sxs-lookup"><span data-stu-id="1c298-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="1c298-384">Problema: O Google Chrome não está disponível como uma opção de execução</span><span class="sxs-lookup"><span data-stu-id="1c298-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="1c298-385">Google Chrome não é exibido na lista de navegadores em **executados** sobre o **Home** guia.</span><span class="sxs-lookup"><span data-stu-id="1c298-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="1c298-386">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-386">**Workaround**</span></span>  
> <span data-ttu-id="1c298-387">Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows.</span><span class="sxs-lookup"><span data-stu-id="1c298-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="1c298-388">Como alternativa, inicie o Google Chrome, clique o *personalizar e controle Google Chrome* menu, clique em *opções*e, em seguida, clique em *Make Google Chrome meu navegador padrão*.</span><span class="sxs-lookup"><span data-stu-id="1c298-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="1c298-389">Problema: A caixa de diálogo "Foreign Key" não permite inserir uma chave primária</span><span class="sxs-lookup"><span data-stu-id="1c298-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="1c298-390">O **Foreign Key** caixa de diálogo não permite que você insira o nome da chave primário da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="1c298-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="1c298-391">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-391">**Workaround**</span></span>  
> <span data-ttu-id="1c298-392">Isso é intencional.</span><span class="sxs-lookup"><span data-stu-id="1c298-392">This is intentional.</span></span> <span data-ttu-id="1c298-393">Você não precisará inserir o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="1c298-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="1c298-394">Problema: O botão "Relações" está desabilitado</span><span class="sxs-lookup"><span data-stu-id="1c298-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="1c298-395">O **relacionamentos** botão sob o **tabela** guia o **bancos de dados** espaço de trabalho está desabilitado para bancos de dados do SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="1c298-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="1c298-396">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-396">**Workaround**</span></span>  
> <span data-ttu-id="1c298-397">nenhuma.</span><span class="sxs-lookup"><span data-stu-id="1c298-397">None.</span></span> <span data-ttu-id="1c298-398">O SQL Server Compact não oferece suporte a relações entre tabelas.</span><span class="sxs-lookup"><span data-stu-id="1c298-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="1c298-399">Problema: Consultas SQL parametrizadas lançam exceções</span><span class="sxs-lookup"><span data-stu-id="1c298-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="1c298-400">No SQL Server Compact 4.0, se você não especificar um tipo de dados, como `SqlDbType` ou `DbType` para parâmetros em consultas parametrizadas, uma exceção é lançada quando a consulta é executada.</span><span class="sxs-lookup"><span data-stu-id="1c298-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="1c298-401">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="1c298-401">**Workaround**</span></span>  
> <span data-ttu-id="1c298-402">Definir explicitamente o tipo de dados para parâmetros, como `SqlDbType` ou `DbType`.</span><span class="sxs-lookup"><span data-stu-id="1c298-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="1c298-403">Isso é importante no caso de tipos de dados BLOB (`image` e `ntext`).</span><span class="sxs-lookup"><span data-stu-id="1c298-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="1c298-404">Use o código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c298-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="1c298-405">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="1c298-405">For More Information</span></span>

<span data-ttu-id="1c298-406">Para obter mais informações sobre o WebMatrix Beta 3, consulte os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="1c298-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="1c298-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="1c298-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="1c298-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1c298-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="1c298-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="1c298-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="1c298-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="1c298-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="1c298-411">Todos os direitos reservados.</span><span class="sxs-lookup"><span data-stu-id="1c298-411">All Rights Reserved.</span></span> <span data-ttu-id="1c298-412">[Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="1c298-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
