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
ms.openlocfilehash: 6202cb4d28cc8d77c1f83c8cee8c90892deef26e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34818971"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identidade Scaffold em projetos do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

2.1 e posterior do ASP.NET Core fornece [a identidade do ASP.NET Core](xref:security/authentication/identity) como um [biblioteca de classes do Razor](xref:mvc/razor-pages/ui-class). Aplicativos que incluem a identidade podem aplicar o scaffolder para adicionar seletivamente o código-fonte contido na biblioteca de classe a identidade Razor (RCL). Deseja gerar o código-fonte para que você pode modificar o código e alterar o comportamento. Por exemplo, você pode instruir o scaffolder para gerar o código usado no registro. Código gerado tem precedência sobre o mesmo código em RCL a identidade.

Aplicativos que **não** incluem autenticação pode aplicar o scaffolder para adicionar o pacote de identidade RCL. Você tem a opção de selecionar o código de identidade a ser gerado.

Embora o scaffolder gera a maior parte do código necessário, você precisará atualizar seu projeto para concluir o processo. Este documento explica as etapas necessárias para concluir a atualização da estrutura de identidade.

Quando o scaffolder de identidade é executado, uma *ScaffoldingReadme.txt* arquivo é criado no diretório do projeto. O *ScaffoldingReadme.txt* arquivo contém instruções gerais sobre o que é necessário para concluir a atualização da estrutura de identidade. Este documento contém instruções mais completas além de *ScaffoldingReadme.txt* arquivo.

É recomendável usar um sistema de controle de origem que mostra as diferenças de arquivo e permite que você faça alterações fora. Inspecione as alterações depois de executar o scaffolder de identidade.

## <a name="scaffold-identity-into-an-empty-project"></a>Scaffold identidade em um projeto vazio

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Adicione as seguintes chamadas realçadas para o `Startup` classe:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Scaffold identidade em um projeto do Razor sem autorização existente

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

Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*. Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>As migrações, UseAuthentication e layout

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

No `Configure` método o `Startup` classe, chame [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Alterações de layout

Opcional: Adicionar o logon parcial (`_LoginPartial`) para o arquivo de layout:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Scaffold identidade em um projeto do Razor com autorização

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Algumas opções de identidade são configuradas no *Areas/Identity/IdentityHostingStartup.cs*. Para obter mais informações, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Scaffold identidade em um projeto MVC sem autorização existente

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

Opcional: Adicionar o logon parcial (`_LoginPartial`) para o *Views/Shared/_Layout.cshtml* arquivo:

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Mover o *Pages/Shared/_LoginPartial.cshtml* o arquivo para *Views/Shared/_LoginPartial.cshtml*

Identidade está configurada no *Areas/Identity/IdentityHostingStartup.cs*. Para obter mais informações, consulte IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Chamar [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) depois `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Scaffold identidade em um projeto MVC com autorização

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Excluir o *páginas/compartilhado* pasta e os arquivos nessa pasta.
