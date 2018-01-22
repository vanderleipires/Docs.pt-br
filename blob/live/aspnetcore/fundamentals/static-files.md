---
title: "Trabalhar com arquivos estáticos no núcleo do ASP.NET"
author: rick-anderson
description: "Aprenda a atender e proteger arquivos estáticos e configurar arquivos estáticos hospedagem comportamentos de middleware em um aplicativo web do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="9eb69-103">Trabalhar com arquivos estáticos no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9eb69-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="9eb69-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="9eb69-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="9eb69-105">Arquivos estáticos, como HTML, CSS, imagens e JavaScript, estão ativos que atua como um aplicativo do ASP.NET Core diretamente para clientes.</span><span class="sxs-lookup"><span data-stu-id="9eb69-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="9eb69-106">Algumas etapas de configuração é necessária para habilitar o fornecimento desses arquivos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="9eb69-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9eb69-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="9eb69-108">Servir arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="9eb69-108">Serve static files</span></span>

<span data-ttu-id="9eb69-109">Arquivos estáticos são armazenados no diretório de raiz do projeto da web.</span><span class="sxs-lookup"><span data-stu-id="9eb69-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="9eb69-110">O diretório padrão é  *\<content_root > / wwwroot*, mas pode ser alterada por meio de [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) método.</span><span class="sxs-lookup"><span data-stu-id="9eb69-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="9eb69-111">Consulte [conteúdo raiz](xref:fundamentals/index#content-root) e [raiz Web](xref:fundamentals/index#web-root) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="9eb69-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="9eb69-112">O host do aplicativo da web deve ser informado do diretório raiz de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="9eb69-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9eb69-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9eb69-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9eb69-114">O `WebHost.CreateDefaultBuilder` método define a raiz de conteúdo para o diretório atual:</span><span class="sxs-lookup"><span data-stu-id="9eb69-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9eb69-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9eb69-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9eb69-116">Definir a raiz de conteúdo para o diretório atual invocando [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) dentro de `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="9eb69-117">Arquivos estáticos são acessíveis por meio de um caminho relativo à raiz da web.</span><span class="sxs-lookup"><span data-stu-id="9eb69-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="9eb69-118">Por exemplo, o **aplicativo Web** várias pastas dentro do modelo de projeto contém o *wwwroot* pasta:</span><span class="sxs-lookup"><span data-stu-id="9eb69-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="9eb69-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="9eb69-119">**wwwroot**</span></span>
  * <span data-ttu-id="9eb69-120">**css**</span><span class="sxs-lookup"><span data-stu-id="9eb69-120">**css**</span></span>
  * <span data-ttu-id="9eb69-121">**images**</span><span class="sxs-lookup"><span data-stu-id="9eb69-121">**images**</span></span>
  * <span data-ttu-id="9eb69-122">**js**</span><span class="sxs-lookup"><span data-stu-id="9eb69-122">**js**</span></span>

<span data-ttu-id="9eb69-123">O formato URI para acessar um arquivo no *imagens* subpasta é *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="9eb69-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="9eb69-124">Por exemplo, *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="9eb69-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9eb69-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9eb69-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9eb69-126">Se o destino do .NET Framework, adicione o [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacote ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9eb69-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="9eb69-127">Se o objetivo principal do .NET, o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) inclui esse pacote.</span><span class="sxs-lookup"><span data-stu-id="9eb69-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9eb69-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9eb69-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9eb69-129">Adicionar o [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacote ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9eb69-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="9eb69-130">Configurar o [middleware](xref:fundamentals/middleware) que permite o fornecimento de arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="9eb69-131">Serve arquivos dentro da raiz da web</span><span class="sxs-lookup"><span data-stu-id="9eb69-131">Serve files inside of web root</span></span>

<span data-ttu-id="9eb69-132">Invocar o [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método dentro de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="9eb69-133">Sem o parâmetro `UseStaticFiles` sobrecarga do método marca os arquivos na raiz da web como servable.</span><span class="sxs-lookup"><span data-stu-id="9eb69-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="9eb69-134">As seguintes referências de marcação *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="9eb69-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="9eb69-135">Serve arquivos fora da raiz da web</span><span class="sxs-lookup"><span data-stu-id="9eb69-135">Serve files outside of web root</span></span>

<span data-ttu-id="9eb69-136">Considere uma hierarquia de diretórios na qual os arquivos estáticos sejam atendidos residam fora da raiz da web:</span><span class="sxs-lookup"><span data-stu-id="9eb69-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="9eb69-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="9eb69-137">**wwwroot**</span></span>
  * <span data-ttu-id="9eb69-138">**css**</span><span class="sxs-lookup"><span data-stu-id="9eb69-138">**css**</span></span>
  * <span data-ttu-id="9eb69-139">**images**</span><span class="sxs-lookup"><span data-stu-id="9eb69-139">**images**</span></span>
  * <span data-ttu-id="9eb69-140">**js**</span><span class="sxs-lookup"><span data-stu-id="9eb69-140">**js**</span></span>
* <span data-ttu-id="9eb69-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="9eb69-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="9eb69-142">**images**</span><span class="sxs-lookup"><span data-stu-id="9eb69-142">**images**</span></span>
      * <span data-ttu-id="9eb69-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="9eb69-143">*banner1.svg*</span></span>

<span data-ttu-id="9eb69-144">Uma solicitação pode acessar o *banner1.svg* arquivo Configurando o middleware de arquivo estático da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9eb69-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="9eb69-145">No código anterior, o *MyStaticFiles* hierarquia de diretórios é exposta publicamente por meio de *StaticFiles* segmento do URI.</span><span class="sxs-lookup"><span data-stu-id="9eb69-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="9eb69-146">Uma solicitação para *http://\<server_address > /StaticFiles/images/banner1.svg* serve a *banner1.svg* arquivo.</span><span class="sxs-lookup"><span data-stu-id="9eb69-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="9eb69-147">As seguintes referências de marcação *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="9eb69-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="9eb69-148">Definir cabeçalhos de resposta HTTP</span><span class="sxs-lookup"><span data-stu-id="9eb69-148">Set HTTP response headers</span></span>

<span data-ttu-id="9eb69-149">Um [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objeto pode ser usado para definir cabeçalhos de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9eb69-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="9eb69-150">Além de configurar os serviços de arquivo estático da raiz da web, o código a seguir define o `Cache-Control` cabeçalho:</span><span class="sxs-lookup"><span data-stu-id="9eb69-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="9eb69-151">O [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método existe o [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pacote.</span><span class="sxs-lookup"><span data-stu-id="9eb69-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="9eb69-152">Os arquivos foram feitos publicamente armazenável em cache por 10 minutos (600 segundos):</span><span class="sxs-lookup"><span data-stu-id="9eb69-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Mostra o cabeçalho Cache-Control de cabeçalhos de resposta foi adicionado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="9eb69-154">Autorização de arquivo estático</span><span class="sxs-lookup"><span data-stu-id="9eb69-154">Static file authorization</span></span>

<span data-ttu-id="9eb69-155">O middleware de arquivo estático não fornece verificações de autorização.</span><span class="sxs-lookup"><span data-stu-id="9eb69-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="9eb69-156">Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot*, podem ser acessados publicamente.</span><span class="sxs-lookup"><span data-stu-id="9eb69-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="9eb69-157">Para servir arquivos com base na autorização:</span><span class="sxs-lookup"><span data-stu-id="9eb69-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="9eb69-158">Armazená-los fora do *wwwroot* e qualquer diretório acessível para o middleware de arquivo estático **e**</span><span class="sxs-lookup"><span data-stu-id="9eb69-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="9eb69-159">Servi-los por meio de um método de ação para o qual a autorização é aplicada.</span><span class="sxs-lookup"><span data-stu-id="9eb69-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="9eb69-160">Retornar um [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objeto:</span><span class="sxs-lookup"><span data-stu-id="9eb69-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="9eb69-161">Habilitar a pesquisa no diretório</span><span class="sxs-lookup"><span data-stu-id="9eb69-161">Enable directory browsing</span></span>

<span data-ttu-id="9eb69-162">Pesquisa no diretório permite que os usuários de seu aplicativo web ver uma lista de pastas e arquivos em um diretório especificado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="9eb69-163">Pesquisa no diretório é desabilitada por padrão por motivos de segurança (consulte [considerações](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="9eb69-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="9eb69-164">Habilitar pesquisa invocando no diretório de [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) método `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="9eb69-165">Adicionar serviços necessários invocando o [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) método de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="9eb69-166">O código anterior permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL *http://\<server_address > / MyImages*, com links para cada arquivo e pasta:</span><span class="sxs-lookup"><span data-stu-id="9eb69-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![pesquisa no diretório](static-files/_static/dir-browse.png)

<span data-ttu-id="9eb69-168">Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa.</span><span class="sxs-lookup"><span data-stu-id="9eb69-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="9eb69-169">Observe os dois `UseStaticFiles` chama no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="9eb69-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="9eb69-170">A primeira chamada permite o fornecimento de arquivos estáticos no *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="9eb69-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="9eb69-171">A segunda chamada permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="9eb69-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="9eb69-172">Fornecer um documento padrão</span><span class="sxs-lookup"><span data-stu-id="9eb69-172">Serve a default document</span></span>

<span data-ttu-id="9eb69-173">Definir uma home page padrão fornece ao visitantes um ponto de partida lógico de ao visitar o site.</span><span class="sxs-lookup"><span data-stu-id="9eb69-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="9eb69-174">Para atender a uma página padrão sem qualificar totalmente o URI do usuário, chame o [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="9eb69-175">`UseDefaultFiles`deve ser chamado antes de `UseStaticFiles` para servir o arquivo padrão.</span><span class="sxs-lookup"><span data-stu-id="9eb69-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="9eb69-176">`UseDefaultFiles`é um regravador de URL que, na verdade, não servem para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="9eb69-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="9eb69-177">Habilitar o middleware de arquivo estático via `UseStaticFiles` para servir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="9eb69-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="9eb69-178">Com `UseDefaultFiles`, solicitações para pesquisar uma pasta:</span><span class="sxs-lookup"><span data-stu-id="9eb69-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="9eb69-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="9eb69-179">*default.htm*</span></span>
* <span data-ttu-id="9eb69-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="9eb69-180">*default.html*</span></span>
* <span data-ttu-id="9eb69-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="9eb69-181">*index.htm*</span></span>
* <span data-ttu-id="9eb69-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="9eb69-182">*index.html*</span></span>

<span data-ttu-id="9eb69-183">O primeiro arquivo encontrado na lista é tratado como se a solicitação fosse o URI totalmente qualificado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="9eb69-184">Continua a URL do navegador refletir o URI solicitado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="9eb69-185">O código a seguir altera o nome de arquivo padrão para *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="9eb69-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="9eb69-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="9eb69-186">UseFileServer</span></span>

<span data-ttu-id="9eb69-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina a funcionalidade de `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="9eb69-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="9eb69-188">O código a seguir permite que o servidor de arquivos estáticos e o arquivo padrão.</span><span class="sxs-lookup"><span data-stu-id="9eb69-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="9eb69-189">Pesquisa no diretório não está habilitada.</span><span class="sxs-lookup"><span data-stu-id="9eb69-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="9eb69-190">O código a seguir baseia-se a sobrecarga sem parâmetros, permitindo a pesquisa no diretório:</span><span class="sxs-lookup"><span data-stu-id="9eb69-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="9eb69-191">Considere a seguinte hierarquia de diretório:</span><span class="sxs-lookup"><span data-stu-id="9eb69-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="9eb69-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="9eb69-192">**wwwroot**</span></span>
  * <span data-ttu-id="9eb69-193">**css**</span><span class="sxs-lookup"><span data-stu-id="9eb69-193">**css**</span></span>
  * <span data-ttu-id="9eb69-194">**images**</span><span class="sxs-lookup"><span data-stu-id="9eb69-194">**images**</span></span>
  * <span data-ttu-id="9eb69-195">**js**</span><span class="sxs-lookup"><span data-stu-id="9eb69-195">**js**</span></span>
* <span data-ttu-id="9eb69-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="9eb69-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="9eb69-197">**images**</span><span class="sxs-lookup"><span data-stu-id="9eb69-197">**images**</span></span>
      * <span data-ttu-id="9eb69-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="9eb69-198">*banner1.svg*</span></span>
  * <span data-ttu-id="9eb69-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="9eb69-199">*default.html*</span></span>

<span data-ttu-id="9eb69-200">O código a seguir permite que arquivos estáticos, arquivos padrão e a pesquisa no diretório de `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="9eb69-201">`AddDirectoryBrowser`deve ser chamado quando o `EnableDirectoryBrowsing` é o valor da propriedade `true`:</span><span class="sxs-lookup"><span data-stu-id="9eb69-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="9eb69-202">URLs de código usando a hierarquia de arquivos e o anterior, resolver da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="9eb69-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="9eb69-203">URI</span><span class="sxs-lookup"><span data-stu-id="9eb69-203">URI</span></span>            |                             <span data-ttu-id="9eb69-204">Resposta</span><span class="sxs-lookup"><span data-stu-id="9eb69-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="9eb69-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="9eb69-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="9eb69-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="9eb69-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="9eb69-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="9eb69-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="9eb69-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="9eb69-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="9eb69-209">Se nenhum arquivo nomeados padrão existe o *MyStaticFiles* diretório, *http://\<server_address > / StaticFiles* retorna o diretório listando com links clicáveis:</span><span class="sxs-lookup"><span data-stu-id="9eb69-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Lista de arquivos estáticos](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="9eb69-211">`UseDefaultFiles`e `UseDirectoryBrowser` usar a URL *http://\<server_address > / StaticFiles* sem a barra à direita para disparar um cliente redirecionar *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="9eb69-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="9eb69-212">Observe a adição da barra à direita.</span><span class="sxs-lookup"><span data-stu-id="9eb69-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="9eb69-213">URLs relativas dentro dos documentos são considerados inválidos sem uma barra à direita.</span><span class="sxs-lookup"><span data-stu-id="9eb69-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="9eb69-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="9eb69-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="9eb69-215">O [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) classe contém um `Mappings` propriedade servindo como um mapeamento de extensões de arquivo para tipos de conteúdo MIME.</span><span class="sxs-lookup"><span data-stu-id="9eb69-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="9eb69-216">No exemplo a seguir, são registradas várias extensões de arquivo para os tipos MIME conhecidos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="9eb69-217">O *. rtf* extensão é substituída, e *. mp4* é removido.</span><span class="sxs-lookup"><span data-stu-id="9eb69-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="9eb69-218">Consulte [tipos de conteúdo MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="9eb69-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="9eb69-219">Tipos de conteúdo não padrão</span><span class="sxs-lookup"><span data-stu-id="9eb69-219">Non-standard content types</span></span>

<span data-ttu-id="9eb69-220">O middleware de arquivo estático compreende quase 400 tipos de conteúdo de arquivo conhecidos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="9eb69-221">Se o usuário solicita um arquivo de um tipo de arquivo desconhecido, o middleware de arquivo estático retorna uma resposta HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="9eb69-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="9eb69-222">Se a pesquisa no diretório estiver habilitada, será exibido um link para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="9eb69-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="9eb69-223">O URI retorna um erro HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="9eb69-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="9eb69-224">O código a seguir habilita atendendo tipos desconhecidos e renderiza o arquivo desconhecido como uma imagem:</span><span class="sxs-lookup"><span data-stu-id="9eb69-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="9eb69-225">Com o código anterior, uma solicitação para um arquivo com um tipo de conteúdo desconhecido é retornada como uma imagem.</span><span class="sxs-lookup"><span data-stu-id="9eb69-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="9eb69-226">Habilitando [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) é um risco de segurança.</span><span class="sxs-lookup"><span data-stu-id="9eb69-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="9eb69-227">Ele é desabilitado por padrão, e seu uso não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="9eb69-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) fornece uma alternativa mais segura para servir arquivos com extensões não padrão.</span><span class="sxs-lookup"><span data-stu-id="9eb69-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="9eb69-229">Considerações</span><span class="sxs-lookup"><span data-stu-id="9eb69-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="9eb69-230">`UseDirectoryBrowser`e `UseStaticFiles` podem vazar segredos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="9eb69-231">Desabilitar pesquisa na produção no diretório é altamente recomendável.</span><span class="sxs-lookup"><span data-stu-id="9eb69-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="9eb69-232">Leia com atenção os diretórios que são habilitados por meio de `UseStaticFiles` ou `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="9eb69-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="9eb69-233">Todo o diretório e seus subdiretórios se tornar acessíveis publicamente.</span><span class="sxs-lookup"><span data-stu-id="9eb69-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="9eb69-234">Armazenamento de arquivos adequado para serviços ao público em um diretório dedicado, como  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9eb69-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="9eb69-235">Esses arquivos separados de exibições do MVC, páginas Razor (2. x somente), arquivos de configuração, etc.</span><span class="sxs-lookup"><span data-stu-id="9eb69-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="9eb69-236">As URLs para conteúdo exposto com `UseDirectoryBrowser` e `UseStaticFiles` estão sujeitos a diferenciação de maiusculas e minúsculas e restrições de caracteres do sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="9eb69-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="9eb69-237">Por exemplo, Windows diferencia maiusculas de minúsculas&mdash;Mac e Linux não são.</span><span class="sxs-lookup"><span data-stu-id="9eb69-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="9eb69-238">Aplicativos do ASP.NET Core hospedados no IIS use o [ASP.NET Core módulo (ANCM)](xref:fundamentals/servers/aspnet-core-module) para encaminhar todas as solicitações para o aplicativo, inclusive solicitações de arquivo estático.</span><span class="sxs-lookup"><span data-stu-id="9eb69-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="9eb69-239">O manipulador de arquivo estático do IIS não é usado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="9eb69-240">Ele tem nenhuma possibilidade de manipular solicitações antes que eles são manipulados pelo ANCM.</span><span class="sxs-lookup"><span data-stu-id="9eb69-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="9eb69-241">Conclua as etapas a seguir no Gerenciador do IIS para remover o manipulador de arquivo estático no IIS no nível do servidor ou site:</span><span class="sxs-lookup"><span data-stu-id="9eb69-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="9eb69-242">Navegue até o **módulos** recurso.</span><span class="sxs-lookup"><span data-stu-id="9eb69-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="9eb69-243">Selecione **StaticFileModule** na lista.</span><span class="sxs-lookup"><span data-stu-id="9eb69-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="9eb69-244">Clique em **remover** no **ações** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="9eb69-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="9eb69-245">Se o manipulador de arquivo estático no IIS é habilitado **e** o ANCM está configurado incorretamente, arquivos estáticos são atendidos.</span><span class="sxs-lookup"><span data-stu-id="9eb69-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="9eb69-246">Isso acontece, por exemplo, se o *Web. config* arquivo não está implantado.</span><span class="sxs-lookup"><span data-stu-id="9eb69-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="9eb69-247">Colocar arquivos de código (incluindo *. CS* e *. cshtml*) fora da aplicativo raiz do projeto da web.</span><span class="sxs-lookup"><span data-stu-id="9eb69-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="9eb69-248">Uma separação lógica, portanto, é criada entre o aplicativo cliente conteúdo e código de servidor.</span><span class="sxs-lookup"><span data-stu-id="9eb69-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="9eb69-249">Isso impede que o código do lado do servidor vazem.</span><span class="sxs-lookup"><span data-stu-id="9eb69-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9eb69-250">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="9eb69-250">Additional resources</span></span>

* [<span data-ttu-id="9eb69-251">Middleware</span><span class="sxs-lookup"><span data-stu-id="9eb69-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="9eb69-252">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9eb69-252">Introduction to ASP.NET Core</span></span>](xref:index)
