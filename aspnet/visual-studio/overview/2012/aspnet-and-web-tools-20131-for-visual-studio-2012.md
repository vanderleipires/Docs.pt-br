---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de versão do ASP.NET and Web Tools 2013.1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Este documento descreve a versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012.
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829095"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="68249-103">Notas de versão do ASP.NET and Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="68249-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="68249-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="68249-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="68249-105">Este documento descreve a versão do ASP.NET e Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="68249-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="68249-106">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="68249-106">Contents</span></span>

- [<span data-ttu-id="68249-107">Notas de instalação</span><span class="sxs-lookup"><span data-stu-id="68249-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="68249-108">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="68249-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="68249-109">Novos recursos no ASP.NET e Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="68249-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="68249-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="68249-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="68249-111">Modelos</span><span class="sxs-lookup"><span data-stu-id="68249-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="68249-112">Modelo do ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="68249-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="68249-113">Modelo de API Web 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="68249-114">Modelos de item</span><span class="sxs-lookup"><span data-stu-id="68249-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="68249-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="68249-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="68249-116">Scaffolding do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="68249-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="68249-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="68249-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="68249-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="68249-119">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="68249-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="68249-120">Scaffolding do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="68249-121">MVC e o Scaffolding de API Web - HTTP 404 não encontrado erro</span><span class="sxs-lookup"><span data-stu-id="68249-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="68249-122">Visual Studio Express 2012 para Web para de funcionar após a adição de um item de scaffolding</span><span class="sxs-lookup"><span data-stu-id="68249-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="68249-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="68249-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="68249-124">Exibir o arquivo cshtml com procurar com ou F5 causa um erro de servidor</span><span class="sxs-lookup"><span data-stu-id="68249-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="68249-125">Tilde(~) e reconfiguração de Url</span><span class="sxs-lookup"><span data-stu-id="68249-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="68249-126">Modelos</span><span class="sxs-lookup"><span data-stu-id="68249-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="68249-127">Notas de instalação</span><span class="sxs-lookup"><span data-stu-id="68249-127">Installation Notes</span></span>

<span data-ttu-id="68249-128">[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET e Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="68249-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="68249-129">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="68249-129">Software Requirements</span></span>

<span data-ttu-id="68249-130">Você deve ter o Visual Studio 2012 ou Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="68249-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="68249-131">Novos recursos no ASP.NET e Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="68249-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="68249-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="68249-132">Bootstrap</span></span>

<span data-ttu-id="68249-133">Quando você criar o scaffolding MVC 5 controladores e modos de exibição, a marcação para os modos de exibição usa [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="68249-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="68249-134">Modelos</span><span class="sxs-lookup"><span data-stu-id="68249-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="68249-135">Modelo do ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="68249-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="68249-136">Adicionamos um novo modelo MVC 5.</span><span class="sxs-lookup"><span data-stu-id="68249-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="68249-137">Ele faz referência os últimos pacotes NuGet do MVC 5, e você pode usar o scaffolding para adicionar controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="68249-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="68249-138">Modelo de API Web 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="68249-139">Adicionamos um novo modelo de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="68249-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="68249-140">Ele faz referência os últimos pacotes NuGet da API Web 2, e você pode usar o scaffolding para adicionar controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="68249-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="68249-141">Modelos de item</span><span class="sxs-lookup"><span data-stu-id="68249-141">Item Templates</span></span>

<span data-ttu-id="68249-142">Adicionamos novos modelos de item para modos de exibição MVC 5, páginas da Web (Razor 3) e controladores de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="68249-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="68249-143">Instalar os pacotes do NuGet relacionados ao projeto durante a adição de novos itens.</span><span class="sxs-lookup"><span data-stu-id="68249-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="68249-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="68249-144">Entity Framework 6</span></span>

<span data-ttu-id="68249-145">Quando você criar o scaffolding de um controlador MVC ou API da Web usando o Entity Framework, usamos Framework 6.</span><span class="sxs-lookup"><span data-stu-id="68249-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="68249-146">Para obter mais informações sobre o Entity Framework, consulte o [histórico de versão do Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="68249-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="68249-147">Você também pode baixar e instalar as ferramentas do Entity Framework 6 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="68249-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="68249-148">Consulte a [obter o Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="68249-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="68249-149">Scaffolding do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="68249-150">Scaffolding do ASP.NET é uma estrutura de geração de código para aplicativos Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68249-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="68249-151">Ele torna mais fácil adicionar o código clichê ao seu projeto que interage com um modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="68249-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="68249-152">Nas versões anteriores do Visual Studio, o scaffolding foi limitado a projetos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="68249-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="68249-153">Com essa atualização, agora você pode usar o scaffolding para qualquer projeto ASP.NET, incluindo o Web Forms.</span><span class="sxs-lookup"><span data-stu-id="68249-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="68249-154">Esta atualização não oferece suporte a geração de páginas para um projeto de Web Forms, mas você ainda pode usar o scaffolding com Web Forms Adicionando dependências MVC ao projeto.</span><span class="sxs-lookup"><span data-stu-id="68249-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="68249-155">Suporte para a geração de páginas para formulários da Web será adicionado em uma atualização futura.</span><span class="sxs-lookup"><span data-stu-id="68249-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="68249-156">Ao usar o scaffolding, podemos garantir que todos os dependências estão instaladas no projeto.</span><span class="sxs-lookup"><span data-stu-id="68249-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="68249-157">Por exemplo, se você começa com um projeto de Web Forms do ASP.NET e, em seguida, utilizar o scaffolding para adicionar um controlador da API da Web, os pacotes NuGet necessários e as referências são adicionadas ao seu projeto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="68249-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="68249-158">Para adicionar o scaffolding do MVC para um projeto de Web Forms, adicione uma **Novo Item de Scaffold** e selecione **dependências do MVC 5** na janela de diálogo.</span><span class="sxs-lookup"><span data-stu-id="68249-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="68249-159">Há duas opções para scaffolding MVC; Mínimo e completo.</span><span class="sxs-lookup"><span data-stu-id="68249-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="68249-160">Se você selecionar no mínimo, apenas os pacotes do NuGet e referências para o ASP.NET MVC são adicionadas ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="68249-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="68249-161">Se você selecionar a opção completa, as dependências mínimas são adicionadas, bem como os arquivos de conteúdo necessários para um projeto do MVC.</span><span class="sxs-lookup"><span data-stu-id="68249-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="68249-162">Suporte para scaffolding dos controladores assíncronos usa os novos recursos assíncronos do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="68249-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="68249-163">Para obter mais informações e tutoriais, consulte [visão geral de Scaffolding do ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68249-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="68249-164">Esses tutoriais mostram scaffolding com o Visual Studio 2013, mas eles também são aplicáveis para o ASP.NET e Web Tools 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="68249-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="68249-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="68249-165">Razor Editor</span></span>

<span data-ttu-id="68249-166">Com essa atualização, o Visual Studio 2012 agora dá suporte a ferramentas/edição de Razor 3.</span><span class="sxs-lookup"><span data-stu-id="68249-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="68249-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="68249-167">NuGet 2.7</span></span>

<span data-ttu-id="68249-168">NuGet 2.7 inclui um conjunto avançado de novos recursos que são descritas detalhadamente no [notas de versão do NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="68249-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="68249-169">Esta versão do NuGet remove a necessidade de usuários permitir explicitamente que o NuGet para restaurar pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="68249-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="68249-170">Ao instalar o NuGet 2.7, os usuários implicitamente dar consentimento para restaurar automaticamente os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="68249-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="68249-171">Os usuários podem recusar explicitamente restauração de pacote por meio das configurações do NuGet no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68249-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="68249-172">Essa alteração simplifica como funciona a restauração de pacote.</span><span class="sxs-lookup"><span data-stu-id="68249-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="68249-173">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="68249-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="68249-174">Scaffolding do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68249-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="68249-175">MVC e o Scaffolding de API Web - HTTP 404 não encontrado erro</span><span class="sxs-lookup"><span data-stu-id="68249-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="68249-176">Se você encontrar um erro ao adicionar um item de scaffolding para um projeto, é possível que seu projeto será deixado em um estado inconsistente.</span><span class="sxs-lookup"><span data-stu-id="68249-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="68249-177">Algumas das alterações feitas a ser scaffolding serão revertidas, mas outras alterações, como os pacotes NuGet instalados, não serão revertidas novamente.</span><span class="sxs-lookup"><span data-stu-id="68249-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="68249-178">Se as alterações de configuração de roteamento são revertidas, os usuários receberá um erro HTTP 404 quando navegar até geradas por scaffolding itens.</span><span class="sxs-lookup"><span data-stu-id="68249-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="68249-179">Para corrigir esse erro para MVC, adicione um novo item com Scaffold e selecione as dependências do MVC 5 (mínimo ou completa).</span><span class="sxs-lookup"><span data-stu-id="68249-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="68249-180">Esse processo adicionará todas as alterações necessárias ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="68249-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="68249-181">Para corrigir esse erro para a API da Web:</span><span class="sxs-lookup"><span data-stu-id="68249-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="68249-182">Adicione a seguinte classe WebApiConfig ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="68249-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="68249-183">Configurar Webapiconfig no aplicativo\_inicie método no global. asax, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="68249-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="68249-184">Visual Studio Express 2012 para Web para de funcionar após a adição de um item de scaffolding</span><span class="sxs-lookup"><span data-stu-id="68249-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="68249-185">Se o Visual Studio Express 2012 para Web parar de funcionar após a adição de item de scaffolding com o Entity Framework (por exemplo, o controlador Web API 2 com ações, usando o Entity Framework), é possível que o Visual Studio Express não pôde carregar a imagem nativa de um assembly Dependendo do Extensions.</span><span class="sxs-lookup"><span data-stu-id="68249-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="68249-186">Para corrigir esse problema, configure o Visual Studio Express para trabalhar com a imagem MSIL de Extensions:</span><span class="sxs-lookup"><span data-stu-id="68249-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="68249-187">Abra o Prompt de comando no modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="68249-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="68249-188">Vá para %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ou % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (para 64 bits Windows).</span><span class="sxs-lookup"><span data-stu-id="68249-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="68249-189">Abra VWDExpress.exe.config em um editor de texto.</span><span class="sxs-lookup"><span data-stu-id="68249-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="68249-190">Adicione a seguinte linha sob o &lt;configuration&gt;/&lt;runtime&gt; elemento:</span><span class="sxs-lookup"><span data-stu-id="68249-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="68249-191">Reinicie o Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="68249-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="68249-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="68249-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="68249-193">Exibindo cshtml arquivo withBrowse WithorF5causes um erro de servidor</span><span class="sxs-lookup"><span data-stu-id="68249-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="68249-194">Quando você cria um projeto MVC 5 no Visual Studio 2012 (ou abrir no projeto do Visual Studio 2012 um MVC 5 que foi criado no Visual Studio 2013) e tenta exibir um arquivo cshtml usando Procurar com ou F5, você receberá um erro que afirma - **erro Server Aplicativo '/'**.</span><span class="sxs-lookup"><span data-stu-id="68249-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="68249-195">O servidor tenta navegar para `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="68249-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="68249-196">Para resolver esse problema, altere a **iniciar ação** definindo em seu projeto para **página específica**.</span><span class="sxs-lookup"><span data-stu-id="68249-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="68249-197">Você não precisa fornecer um valor para a página.</span><span class="sxs-lookup"><span data-stu-id="68249-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="68249-198">Depois de fazer essa alteração, selecionar F5 navega para a raiz do seu aplicativo (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="68249-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="68249-199">Esse comportamento não é o mesmo que o comportamento para projetos MVC 5 no Visual Studio 2013, em que o **página atual** configuração inicia a página aberta.</span><span class="sxs-lookup"><span data-stu-id="68249-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="68249-200">Tilde(~) e reconfiguração de Url</span><span class="sxs-lookup"><span data-stu-id="68249-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="68249-201">Após a atualização 3 do ASP.NET Razor ou ASP.NET MVC 5, a notação tilde(~) pode não funcionar corretamente se você estiver usando regravações de URL.</span><span class="sxs-lookup"><span data-stu-id="68249-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="68249-202">A reconfiguração de URL afeta a notação tilde(~) em elementos HTML, como &lt;um /&gt;, &lt;SCRIPT /&gt;, &lt;LINK /&gt;, e assim o til não mapeia para o diretório raiz.</span><span class="sxs-lookup"><span data-stu-id="68249-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="68249-203">Por exemplo, se você reescrever as solicitações para **asp.net/content** à **asp.net**, o atributo href &lt;href = "~/content/" /&gt; resolve para **/content/ conteúdo /** em vez de **/**.</span><span class="sxs-lookup"><span data-stu-id="68249-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="68249-204">Para suprimir essa alteração, você pode definir as **IIS\_WasUrlRewritten** contexto como false em cada página da Web ou no **aplicativo\_BeginRequest** no global. asax.</span><span class="sxs-lookup"><span data-stu-id="68249-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="68249-205">Modelos</span><span class="sxs-lookup"><span data-stu-id="68249-205">Templates</span></span>

<span data-ttu-id="68249-206">Quando você cria ASP.NET MVC projetos com o Visual Studio 2012 no Windows 8.1 ou Windows Server 2012 R2, o Visual Studio exibe uma mensagem de erro que afirma "Configurando Web [url] para ASP.NET 4.5 falhou".</span><span class="sxs-lookup"><span data-stu-id="68249-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Erro de configuração](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="68249-208">Você verá esse erro, porque o Visual Studio 2012 não habilita o recurso ASP.NET 4.5 quando ele é instalado nessas versões do Windows.</span><span class="sxs-lookup"><span data-stu-id="68249-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="68249-209">Para habilitar o ASP.NET 4.5, execute as etapas descritas em [ativar ou desativar recursos do Windows ativar](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="68249-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Ativar ou desativar recursos do Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="68249-211">Como alternativa, você pode habilitar o ASP.NET 4.5 por meio da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="68249-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="68249-212">Abra o Prompt de comando no modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="68249-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="68249-213">Execute o seguinte comando para habilitar o ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="68249-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
