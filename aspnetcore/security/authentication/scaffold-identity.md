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
ms.openlocfilehash: 80cd39af61e856d3ce92db1c26e70788bcdca83d
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725813"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="a4a69-103">Identidade Scaffold em projetos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4a69-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="a4a69-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4a69-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4a69-105">2.1 e posterior do ASP.NET Core fornece [a identidade do ASP.NET Core](xref:security/authentication/identity) como um [biblioteca de classes do Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a4a69-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="a4a69-106">Aplicativos que incluem a identidade podem aplicar o scaffolder para adicionar seletivamente o código-fonte contido na biblioteca de classe a identidade Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="a4a69-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="a4a69-107">Deseja gerar o código-fonte para que você pode modificar o código e alterar o comportamento.</span><span class="sxs-lookup"><span data-stu-id="a4a69-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="a4a69-108">Por exemplo, você pode instruir o scaffolder para gerar o código usado no registro.</span><span class="sxs-lookup"><span data-stu-id="a4a69-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="a4a69-109">Código gerado tem precedência sobre o mesmo código em RCL a identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="a4a69-110">Para obter controle total da interface do usuário e não usar o padrão RCL, consulte a seção [criar fonte de interface do usuário de identidade completa](#full).</span><span class="sxs-lookup"><span data-stu-id="a4a69-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="a4a69-111">Aplicativos que **não** incluem autenticação pode aplicar o scaffolder para adicionar o pacote de identidade RCL.</span><span class="sxs-lookup"><span data-stu-id="a4a69-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="a4a69-112">Você tem a opção de selecionar o código de identidade a ser gerado.</span><span class="sxs-lookup"><span data-stu-id="a4a69-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="a4a69-113">Embora o scaffolder gera a maior parte do código necessário, você precisará atualizar seu projeto para concluir o processo.</span><span class="sxs-lookup"><span data-stu-id="a4a69-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="a4a69-114">Este documento explica as etapas necessárias para concluir a atualização da estrutura de identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="a4a69-115">Quando o scaffolder de identidade é executado, uma *ScaffoldingReadme.txt* arquivo é criado no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="a4a69-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="a4a69-116">O *ScaffoldingReadme.txt* arquivo contém instruções gerais sobre o que é necessário para concluir a atualização da estrutura de identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="a4a69-117">Este documento contém instruções mais completas além de *ScaffoldingReadme.txt* arquivo.</span><span class="sxs-lookup"><span data-stu-id="a4a69-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="a4a69-118">É recomendável usar um sistema de controle de origem que mostra as diferenças de arquivo e permite que você faça alterações fora.</span><span class="sxs-lookup"><span data-stu-id="a4a69-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="a4a69-119">Inspecione as alterações depois de executar o scaffolder de identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="a4a69-120">Scaffold identidade em um projeto vazio</span><span class="sxs-lookup"><span data-stu-id="a4a69-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="a4a69-121">Adicione as seguintes chamadas realçadas para o `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="a4a69-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="a4a69-122">Scaffold identidade em um projeto do Razor sem autorização existente</span><span class="sxs-lookup"><span data-stu-id="a4a69-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="a4a69-123">Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a69-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a69-124">Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a4a69-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="a4a69-125">As migrações, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="a4a69-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a4a69-126">No `Configure` método o `Startup` classe, chame [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a4a69-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="a4a69-127">Alterações de layout</span><span class="sxs-lookup"><span data-stu-id="a4a69-127">Layout changes</span></span>

<span data-ttu-id="a4a69-128">Opcional: Adicionar o logon parcial (`_LoginPartial`) para o arquivo de layout:</span><span class="sxs-lookup"><span data-stu-id="a4a69-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="a4a69-129">Scaffold identidade em um projeto do Razor com autorização</span><span class="sxs-lookup"><span data-stu-id="a4a69-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="a4a69-130">Algumas opções de identidade são configuradas no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a69-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a69-131">Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a4a69-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="a4a69-132">Scaffold identidade em um projeto MVC sem autorização existente</span><span class="sxs-lookup"><span data-stu-id="a4a69-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="a4a69-133">Opcional: Adicionar o logon parcial (`_LoginPartial`) para o *Views/Shared/_Layout.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="a4a69-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="a4a69-134">Mover o *Pages/Shared/_LoginPartial.cshtml* o arquivo para *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a4a69-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="a4a69-135">Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a4a69-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a4a69-136">Para obter mais informações, consulte IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="a4a69-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a4a69-137">Chamar [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a4a69-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="a4a69-138">Scaffold identidade em um projeto MVC com autorização</span><span class="sxs-lookup"><span data-stu-id="a4a69-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="a4a69-139">Excluir o *páginas/compartilhado* pasta e os arquivos nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="a4a69-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="a4a69-140">Criar fonte de interface do usuário completa de identidade</span><span class="sxs-lookup"><span data-stu-id="a4a69-140">Create full identity UI source</span></span>

<span data-ttu-id="a4a69-141">Para manter o controle total da interface do usuário de identidade, execute o scaffolder de identidade e selecione **substituir todos os arquivos**.</span><span class="sxs-lookup"><span data-stu-id="a4a69-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="a4a69-142">O seguinte código mostra as alterações para substituir o padrão de identidade da interface do usuário com identidade em um aplicativo web do ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a4a69-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="a4a69-143">Você talvez queira fazer isso para ter controle total sobre a interface do usuário de identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="a4a69-144">O padrão de identidade é substituído no código a seguir: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="a4a69-144">The default Identity is replaced in the following code: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]</span></span>

<span data-ttu-id="a4a69-145">O código a seguir configura o ASP.NET Core para autorizar as páginas de identidade que exigem autorização: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="a4a69-145">The following code configures ASP.NET Core to authorize the Identity pages that require authorization: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]</span></span>

<span data-ttu-id="a4a69-146">O seguinte código define o cookie de identidade para usar o caminho correto de páginas de identidade.</span><span class="sxs-lookup"><span data-stu-id="a4a69-146">The following the code sets the Identity cookie to use the correct Identity pages path.</span></span>
[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="a4a69-147">Registrar um `IEmailSender` implementação, por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a4a69-147">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet4)]