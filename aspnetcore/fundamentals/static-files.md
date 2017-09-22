---
title: "Trabalhando com arquivos estáticos no núcleo do ASP.NET"
author: rick-anderson
description: "Trabalhando com arquivos estáticos"
keywords: "ASP.NET Core, arquivos estáticos, ativos estáticos, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 11457cb8684e98147447303ae4653b74414a11fb
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a><span data-ttu-id="2f005-104">Introdução ao trabalho com arquivos estáticos no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2f005-104">Introduction to working with static files in ASP.NET Core</span></span>

<a name=fundamentals-static-files></a>

<span data-ttu-id="2f005-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f005-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2f005-106">Arquivos estáticos, como HTML, CSS, imagem e JavaScript, estão ativos que pode ser usado por um aplicativo do ASP.NET Core diretamente aos clientes.</span><span class="sxs-lookup"><span data-stu-id="2f005-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

[<span data-ttu-id="2f005-107">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="2f005-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a><span data-ttu-id="2f005-108">Servir arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="2f005-108">Serving static files</span></span>

<span data-ttu-id="2f005-109">Arquivos estáticos geralmente estão localizados no `web root` (*\<conteúdo raiz > / wwwroot*) pasta.</span><span class="sxs-lookup"><span data-stu-id="2f005-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="2f005-110">Consulte [conteúdo raiz](xref:fundamentals/index#content-root) e [raiz Web](xref:fundamentals/index#web-root) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="2f005-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="2f005-111">Você geralmente define a raiz de conteúdo para ser o diretório atual para que seu projeto `web root` estão em desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="2f005-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="2f005-112">Arquivos estáticos podem ser armazenados em qualquer pasta sob o `web root` e acessado com um caminho relativo para essa raiz.</span><span class="sxs-lookup"><span data-stu-id="2f005-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="2f005-113">Por exemplo, quando você cria um projeto de aplicativo Web padrão usando o Visual Studio, há várias pastas criadas dentro de *wwwroot* pasta - *css*, *imagens*, e *js*.</span><span class="sxs-lookup"><span data-stu-id="2f005-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="2f005-114">O URI para acessar uma imagem no *imagens* subpasta:</span><span class="sxs-lookup"><span data-stu-id="2f005-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="2f005-115">Para arquivos estáticos sejam atendidos, você deve configurar o [Middleware](middleware.md) para adicionar arquivos estáticos para o pipeline.</span><span class="sxs-lookup"><span data-stu-id="2f005-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="2f005-116">O middleware de arquivo estático pode ser configurado com a adição de uma dependência no *Microsoft.AspNetCore.StaticFiles* pacote no seu projeto e, em seguida, chamar o `UseStaticFiles` método de extensão de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f005-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="2f005-117">`app.UseStaticFiles();`permite que os arquivos em `web root` (*wwwroot* por padrão) servable.</span><span class="sxs-lookup"><span data-stu-id="2f005-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="2f005-118">Posteriormente, mostrarei como fazer outro conteúdo do diretório servable com `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="2f005-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="2f005-119">Você deve incluir o pacote NuGet "Microsoft.AspNetCore.StaticFiles".</span><span class="sxs-lookup"><span data-stu-id="2f005-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="2f005-120">`web root`assume como padrão o *wwwroot* diretório, mas você pode definir o `web root` diretório com `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="2f005-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="2f005-121">Suponha que você tem uma hierarquia de projeto em que os arquivos estáticos que você deseja atender estão fora de `web root`.</span><span class="sxs-lookup"><span data-stu-id="2f005-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="2f005-122">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2f005-122">For example:</span></span>

* <span data-ttu-id="2f005-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="2f005-123">wwwroot</span></span>
  * <span data-ttu-id="2f005-124">CSS</span><span class="sxs-lookup"><span data-stu-id="2f005-124">css</span></span>
  * <span data-ttu-id="2f005-125">imagens</span><span class="sxs-lookup"><span data-stu-id="2f005-125">images</span></span>
  * <span data-ttu-id="2f005-126">...</span><span class="sxs-lookup"><span data-stu-id="2f005-126">...</span></span>
* <span data-ttu-id="2f005-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="2f005-127">MyStaticFiles</span></span>
  * <span data-ttu-id="2f005-128">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="2f005-128">test.png</span></span>

<span data-ttu-id="2f005-129">Para uma solicitação de acesso *test.png*, configure o middleware de arquivos estáticos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2f005-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="2f005-130">Uma solicitação para `http://<app>/StaticFiles/test.png` servirá o *test.png* arquivo.</span><span class="sxs-lookup"><span data-stu-id="2f005-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="2f005-131">`StaticFileOptions()`pode definir cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="2f005-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="2f005-132">Por exemplo, o código a seguir define a servir de arquivos estáticos a *wwwroot* pasta e define o `Cache-Control` cabeçalho para torná-los publicamente armazenável em cache por 10 minutos (600 segundos):</span><span class="sxs-lookup"><span data-stu-id="2f005-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Mostra o cabeçalho Cache-Control de cabeçalhos de resposta foi adicionado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="2f005-134">Autorização de arquivo estático</span><span class="sxs-lookup"><span data-stu-id="2f005-134">Static file authorization</span></span>

<span data-ttu-id="2f005-135">O módulo de arquivo estático fornece **sem** verificações de autorização.</span><span class="sxs-lookup"><span data-stu-id="2f005-135">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="2f005-136">Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot* publicamente disponíveis.</span><span class="sxs-lookup"><span data-stu-id="2f005-136">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="2f005-137">Para servir arquivos com base na autorização:</span><span class="sxs-lookup"><span data-stu-id="2f005-137">To serve files based on authorization:</span></span>

* <span data-ttu-id="2f005-138">Armazená-los fora do *wwwroot* e qualquer diretório acessível para o middleware de arquivo estático **e**</span><span class="sxs-lookup"><span data-stu-id="2f005-138">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="2f005-139">Servi-los por meio de uma ação do controlador, retornando um `FileResult` onde a autorização é aplicada</span><span class="sxs-lookup"><span data-stu-id="2f005-139">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="2f005-140">Habilitando a pesquisa no diretório</span><span class="sxs-lookup"><span data-stu-id="2f005-140">Enabling directory browsing</span></span>

<span data-ttu-id="2f005-141">Pesquisa no diretório permite que o usuário de seu aplicativo web ver uma lista de diretórios e arquivos em um diretório especificado.</span><span class="sxs-lookup"><span data-stu-id="2f005-141">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="2f005-142">Pesquisa no diretório é desabilitada por padrão por motivos de segurança (consulte [considerações](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="2f005-142">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="2f005-143">Para habilitar a pesquisa no diretório, chamar o `UseDirectoryBrowser` método de extensão de `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2f005-143">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="2f005-144">E adicionar os serviços necessários chamando `AddDirectoryBrowser` método de extensão de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f005-144">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="2f005-145">O código acima permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL http://\<aplicativo > / MyImages, com links para cada arquivo e pasta:</span><span class="sxs-lookup"><span data-stu-id="2f005-145">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![pesquisa no diretório](static-files/_static/dir-browse.png)

<span data-ttu-id="2f005-147">Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa.</span><span class="sxs-lookup"><span data-stu-id="2f005-147">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="2f005-148">Observe os dois `app.UseStaticFiles` chamadas.</span><span class="sxs-lookup"><span data-stu-id="2f005-148">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="2f005-149">O primeiro deles é necessário para atender a CSS, imagens e JavaScript no *wwwroot* pasta e a segunda chamada para a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL http://\<aplicativo > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="2f005-149">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="2f005-150">Atendendo a um documento padrão</span><span class="sxs-lookup"><span data-stu-id="2f005-150">Serving a default document</span></span>

<span data-ttu-id="2f005-151">Definir uma home page padrão fornece os visitantes do site um ponto de partida ao visitar o site.</span><span class="sxs-lookup"><span data-stu-id="2f005-151">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="2f005-152">Para seu aplicativo Web atender a uma página padrão sem ter que qualificar totalmente o URI do usuário, chamar o `UseDefaultFiles` método de extensão de `Startup.Configure` da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="2f005-152">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="2f005-153">`UseDefaultFiles`deve ser chamado antes de `UseStaticFiles` para servir o arquivo padrão.</span><span class="sxs-lookup"><span data-stu-id="2f005-153">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="2f005-154">`UseDefaultFiles`é um gravador de nova URL que, na verdade, não servem para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="2f005-154">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="2f005-155">Você deve habilitar o middleware de arquivo estático (`UseStaticFiles`) para servir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="2f005-155">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="2f005-156">Com `UseDefaultFiles`, solicitações em uma pasta pesquisará:</span><span class="sxs-lookup"><span data-stu-id="2f005-156">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="2f005-157">default.htm</span><span class="sxs-lookup"><span data-stu-id="2f005-157">default.htm</span></span>
* <span data-ttu-id="2f005-158">Default</span><span class="sxs-lookup"><span data-stu-id="2f005-158">default.html</span></span>
* <span data-ttu-id="2f005-159">index.htm</span><span class="sxs-lookup"><span data-stu-id="2f005-159">index.htm</span></span>
* <span data-ttu-id="2f005-160">index</span><span class="sxs-lookup"><span data-stu-id="2f005-160">index.html</span></span>

<span data-ttu-id="2f005-161">O primeiro arquivo encontrado na lista será disponibilizado como se a solicitação foi o URI totalmente qualificado (embora a URL do navegador continuarão mostrando o URI solicitado).</span><span class="sxs-lookup"><span data-stu-id="2f005-161">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="2f005-162">O código a seguir mostra como alterar o nome de arquivo padrão para *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="2f005-162">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="2f005-163">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="2f005-163">UseFileServer</span></span>

<span data-ttu-id="2f005-164">`UseFileServer`combina a funcionalidade de `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="2f005-164">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="2f005-165">O código a seguir permite que arquivos estáticos e o arquivo padrão a ser servido, mas não permite a pesquisa no diretório:</span><span class="sxs-lookup"><span data-stu-id="2f005-165">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="2f005-166">O código a seguir permite que arquivos estáticos, arquivos padrão e a pesquisa no diretório:</span><span class="sxs-lookup"><span data-stu-id="2f005-166">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="2f005-167">Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa.</span><span class="sxs-lookup"><span data-stu-id="2f005-167">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="2f005-168">Assim como acontece com `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`, se você deseja servir arquivos que existem fora do `web root`, instanciar e configurar um `FileServerOptions` objeto que você passa como um parâmetro para `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="2f005-168">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="2f005-169">Por exemplo, dada a seguinte hierarquia de diretório em seu aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="2f005-169">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="2f005-170">wwwroot</span><span class="sxs-lookup"><span data-stu-id="2f005-170">wwwroot</span></span>

  * <span data-ttu-id="2f005-171">CSS</span><span class="sxs-lookup"><span data-stu-id="2f005-171">css</span></span>

  * <span data-ttu-id="2f005-172">imagens</span><span class="sxs-lookup"><span data-stu-id="2f005-172">images</span></span>

  * <span data-ttu-id="2f005-173">...</span><span class="sxs-lookup"><span data-stu-id="2f005-173">...</span></span>

* <span data-ttu-id="2f005-174">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="2f005-174">MyStaticFiles</span></span>

  * <span data-ttu-id="2f005-175">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="2f005-175">test.png</span></span>

  * <span data-ttu-id="2f005-176">Default</span><span class="sxs-lookup"><span data-stu-id="2f005-176">default.html</span></span>

<span data-ttu-id="2f005-177">Usando o exemplo de hierarquia acima, talvez você queira habilitar arquivos estáticos, arquivos padrão e procurando o `MyStaticFiles` directory.</span><span class="sxs-lookup"><span data-stu-id="2f005-177">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="2f005-178">No trecho de código a seguir, que pode ser feito com uma única chamada para `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="2f005-178">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="2f005-179">Se `enableDirectoryBrowsing` é definido como `true` é necessário chamar `AddDirectoryBrowser` método de extensão de `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2f005-179">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="2f005-180">Usando a hierarquia de arquivos e o código acima:</span><span class="sxs-lookup"><span data-stu-id="2f005-180">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="2f005-181">URI</span><span class="sxs-lookup"><span data-stu-id="2f005-181">URI</span></span>            |                             <span data-ttu-id="2f005-182">Resposta</span><span class="sxs-lookup"><span data-stu-id="2f005-182">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="2f005-183">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="2f005-183">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="2f005-184">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="2f005-184">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="2f005-185">Se nenhum padrão chamado arquivos estiverem no *MyStaticFiles* diretório, http://\<aplicativo > / StaticFiles retorna o diretório listando com links clicáveis:</span><span class="sxs-lookup"><span data-stu-id="2f005-185">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Lista de arquivos estáticos](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="2f005-187">`UseDefaultFiles`e `UseDirectoryBrowser` levará a url http://\<aplicativo > / StaticFiles sem a barra à direita e a causa de um lado do cliente redirecionamento para http://\<aplicativo > /StaticFiles/ (Adicionar a barra à direita).</span><span class="sxs-lookup"><span data-stu-id="2f005-187">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="2f005-188">Sem as URLs relativas de barra à direita dentro dos documentos seria incorretas.</span><span class="sxs-lookup"><span data-stu-id="2f005-188">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="2f005-189">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="2f005-189">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="2f005-190">O `FileExtensionContentTypeProvider` classe contém uma coleção que mapeia as extensões de arquivo para tipos de conteúdo MIME.</span><span class="sxs-lookup"><span data-stu-id="2f005-190">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="2f005-191">No exemplo a seguir, várias extensões de arquivo são registradas em tipos MIME conhecidos, "RTF" é substituído e ". mp4" é removido.</span><span class="sxs-lookup"><span data-stu-id="2f005-191">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="2f005-192">Consulte [tipos de conteúdo MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="2f005-192">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="2f005-193">Tipos de conteúdo não padrão</span><span class="sxs-lookup"><span data-stu-id="2f005-193">Non-standard content types</span></span>

<span data-ttu-id="2f005-194">O middleware de arquivo estático ASP.NET compreende quase 400 tipos de conteúdo de arquivo conhecidos.</span><span class="sxs-lookup"><span data-stu-id="2f005-194">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="2f005-195">Se o usuário solicita um arquivo de um tipo de arquivo desconhecido, o middleware de arquivo estático retorna uma resposta HTTP 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="2f005-195">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="2f005-196">Se a pesquisa no diretório estiver habilitada, será exibido um link para o arquivo, mas o URI retornará um erro HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="2f005-196">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="2f005-197">O código a seguir habilita atendendo tipos desconhecidos e processará o arquivo desconhecido como uma imagem.</span><span class="sxs-lookup"><span data-stu-id="2f005-197">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="2f005-198">Com o código acima, uma solicitação para um arquivo com um tipo de conteúdo desconhecido será retornada como uma imagem.</span><span class="sxs-lookup"><span data-stu-id="2f005-198">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="2f005-199">Habilitando `ServeUnknownFileTypes` é um risco de segurança e usá-lo não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="2f005-199">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="2f005-200">`FileExtensionContentTypeProvider`(explicado acima) fornece uma alternativa mais segura para servir arquivos com extensões não padrão.</span><span class="sxs-lookup"><span data-stu-id="2f005-200">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="2f005-201">Considerações</span><span class="sxs-lookup"><span data-stu-id="2f005-201">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="2f005-202">`UseDirectoryBrowser`e `UseStaticFiles` podem vazar segredos.</span><span class="sxs-lookup"><span data-stu-id="2f005-202">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="2f005-203">É recomendável que você **não** directory Habilitar navegação em produção.</span><span class="sxs-lookup"><span data-stu-id="2f005-203">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="2f005-204">Tenha cuidado sobre os diretórios que você habilite com `UseStaticFiles` ou `UseDirectoryBrowser` como o diretório inteiro e todos os subdiretórios estarão acessíveis.</span><span class="sxs-lookup"><span data-stu-id="2f005-204">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="2f005-205">É recomendável manter o conteúdo público em seu próprio diretório, como * \<conteúdo raiz > / wwwroot*, longe de modos de exibição do aplicativo, arquivos de configuração, etc.</span><span class="sxs-lookup"><span data-stu-id="2f005-205">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="2f005-206">As URLs para conteúdo exposto com `UseDirectoryBrowser` e `UseStaticFiles` estão sujeitos a diferenciação de maiusculas e minúsculas e restrições de caracteres de seu sistema de arquivos subjacente.</span><span class="sxs-lookup"><span data-stu-id="2f005-206">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="2f005-207">Por exemplo, o Windows diferencia maiusculas de minúsculas, mas o Mac e Linux não são.</span><span class="sxs-lookup"><span data-stu-id="2f005-207">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="2f005-208">Aplicativos do ASP.NET Core hospedados no IIS usam o módulo do ASP.NET Core para encaminhar todas as solicitações para o aplicativo, incluindo solicitações de arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f005-208">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="2f005-209">O manipulador de arquivo estático do IIS não é usado porque ele não obtém a oportunidade de lidar com solicitações antes que eles são tratados pelo módulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f005-209">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="2f005-210">Para remover o manipulador de arquivo estático do IIS (no nível do servidor ou site):</span><span class="sxs-lookup"><span data-stu-id="2f005-210">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="2f005-211">Navegue até o **módulos** recurso</span><span class="sxs-lookup"><span data-stu-id="2f005-211">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="2f005-212">Selecione **StaticFileModule** na lista</span><span class="sxs-lookup"><span data-stu-id="2f005-212">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="2f005-213">Toque em **remover** no **ações** barra lateral</span><span class="sxs-lookup"><span data-stu-id="2f005-213">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="2f005-214">Se o manipulador de arquivo estático no IIS é habilitado **e** o ASP.NET Core módulo (ANCM) não está configurado corretamente (por exemplo se *Web. config* não foi implantado), arquivos estáticos serão servidos.</span><span class="sxs-lookup"><span data-stu-id="2f005-214">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="2f005-215">Arquivos de código (incluindo c# e Razor) devem ser colocados fora do projeto de aplicativo `web root` (*wwwroot* por padrão).</span><span class="sxs-lookup"><span data-stu-id="2f005-215">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="2f005-216">Isso cria uma separação clara entre o conteúdo do lado cliente do aplicativo e código fonte no lado servidor, que impede que o código no lado servidor vazem.</span><span class="sxs-lookup"><span data-stu-id="2f005-216">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f005-217">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2f005-217">Additional Resources</span></span>

* [<span data-ttu-id="2f005-218">Middleware</span><span class="sxs-lookup"><span data-stu-id="2f005-218">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="2f005-219">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f005-219">Introduction to ASP.NET Core</span></span>](../index.md)
