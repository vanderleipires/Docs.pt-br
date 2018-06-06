---
title: Identidade Scaffold em projetos do ASP.NET Core
author: rick-anderson
description: Saiba como o Scaffold de identidade em um projeto do ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 8933cf9c4063bd94f7f3a9ba53b5372b443eb7c8
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758746"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="d8a20-103">Identidade Scaffold em projetos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8a20-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="d8a20-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8a20-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8a20-105">2.1 e posterior do ASP.NET Core fornece [a identidade do ASP.NET Core](xref:security/authentication/identity) como um [biblioteca de classes do Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d8a20-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="d8a20-106">Aplicativos que incluem a identidade podem aplicar o scaffolder para adicionar seletivamente o código-fonte contido na biblioteca de classe a identidade Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="d8a20-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="d8a20-107">Deseja gerar o código-fonte para que você pode modificar o código e alterar o comportamento.</span><span class="sxs-lookup"><span data-stu-id="d8a20-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="d8a20-108">Por exemplo, você pode instruir o scaffolder para gerar o código usado no registro.</span><span class="sxs-lookup"><span data-stu-id="d8a20-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="d8a20-109">Código gerado tem precedência sobre o mesmo código em RCL a identidade.</span><span class="sxs-lookup"><span data-stu-id="d8a20-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="d8a20-110">Aplicativos que **não** incluem autenticação pode aplicar o scaffolder para adicionar o pacote de identidade RCL.</span><span class="sxs-lookup"><span data-stu-id="d8a20-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="d8a20-111">Você tem a opção de selecionar o código de identidade a ser gerado.</span><span class="sxs-lookup"><span data-stu-id="d8a20-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="d8a20-112">Embora o scaffolder gera a maior parte do código necessário, você precisará atualizar seu projeto para concluir o processo.</span><span class="sxs-lookup"><span data-stu-id="d8a20-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="d8a20-113">Este documento explica as etapas necessárias para concluir a atualização da estrutura de identidade.</span><span class="sxs-lookup"><span data-stu-id="d8a20-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="d8a20-114">Quando o scaffolder de identidade é executado, uma *ScaffoldingReadme.txt* arquivo é criado no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="d8a20-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="d8a20-115">O *ScaffoldingReadme.txt* arquivo contém instruções gerais sobre o que é necessário para concluir a atualização da estrutura de identidade.</span><span class="sxs-lookup"><span data-stu-id="d8a20-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="d8a20-116">Este documento contém instruções mais completas que a leitura do *ScaffoldingReadme.txt* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d8a20-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="d8a20-117">É recomendável usar um sistema de controle de origem que mostra as diferenças de arquivo e permite que você faça alterações fora.</span><span class="sxs-lookup"><span data-stu-id="d8a20-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="d8a20-118">Inspecione as alterações depois de executar o scaffolder de identidade.</span><span class="sxs-lookup"><span data-stu-id="d8a20-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="d8a20-119">Scaffold identidade em um projeto vazio</span><span class="sxs-lookup"><span data-stu-id="d8a20-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8a20-120">Adicione as seguintes chamadas realçadas para o `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="d8a20-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="d8a20-121">Scaffold identidade em um projeto do Razor sem autorização existente</span><span class="sxs-lookup"><span data-stu-id="d8a20-121">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8a20-122">Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8a20-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8a20-123">Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8a20-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="d8a20-124">As migrações, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="d8a20-124">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d8a20-125">No `Configure` método o `Startup` classe, chame [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8a20-125">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="d8a20-126">Alterações de layout</span><span class="sxs-lookup"><span data-stu-id="d8a20-126">Layout changes</span></span>

<span data-ttu-id="d8a20-127">Opcional: Adicionar o logon parcial (`_LoginPartial`) para o arquivo de layout:</span><span class="sxs-lookup"><span data-stu-id="d8a20-127">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="d8a20-128">Scaffold identidade em um projeto do Razor com autorização</span><span class="sxs-lookup"><span data-stu-id="d8a20-128">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="d8a20-129">Algumas opções de identidade são configuradas no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8a20-129">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8a20-130">Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8a20-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="d8a20-131">Scaffold identidade em um projeto MVC sem autorização existente</span><span class="sxs-lookup"><span data-stu-id="d8a20-131">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8a20-132">Opcional: Adicionar o logon parcial (`_LoginPartial`) para o *Views/Shared/_Layout.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="d8a20-132">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="d8a20-133">Mover o *Pages/Shared/_LoginPartial.cshtml* o arquivo para *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d8a20-133">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="d8a20-134">Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8a20-134">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8a20-135">Para obter mais informações, consulte IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="d8a20-135">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d8a20-136">Chamar [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8a20-136">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="d8a20-137">Scaffold identidade em um projeto MVC com autorização</span><span class="sxs-lookup"><span data-stu-id="d8a20-137">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="d8a20-138">Excluir o *páginas/compartilhado* pasta e os arquivos nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="d8a20-138">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
