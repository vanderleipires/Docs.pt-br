---
title: Interface do usuário do Razor reutilizável em bibliotecas de classes com o ASP.NET Core
author: Rick-Anderson
description: Explica como criar a interface do usuário do Razor reutilizável em uma biblioteca de classes.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="15c4b-103">Crie interface do usuário reutilizável usando o projeto de biblioteca de classes Razor no ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15c4b-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="15c4b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="15c4b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="15c4b-105">A exibições, as páginas, os controladores, os modelos de página e os modelos de dados Razor podem ser construídos em uma RCL (Biblioteca de Classes Razor).</span><span class="sxs-lookup"><span data-stu-id="15c4b-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library(RCL).</span></span> <span data-ttu-id="15c4b-106">A RCL pode ser e empacotada e reutilizada.</span><span class="sxs-lookup"><span data-stu-id="15c4b-106">The RCL can be and packaged and reused.</span></span> <span data-ttu-id="15c4b-107">Os aplicativos podem incluir a RCL e substituir as exibições e as páginas que ela contém.</span><span class="sxs-lookup"><span data-stu-id="15c4b-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="15c4b-108">Quando uma exibição, uma exibição parcial ou uma página Razor for encontrada no aplicativo Web e na RCL, a marcação Razor (arquivo *.cshtml*) no aplicativo Web terá precedência.</span><span class="sxs-lookup"><span data-stu-id="15c4b-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="15c4b-109">Este recurso requer o [!INCLUDE[](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="15c4b-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="15c4b-110">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15c4b-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="15c4b-111">Criar uma biblioteca de classes contendo a interface do usuário do Razor</span><span class="sxs-lookup"><span data-stu-id="15c4b-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15c4b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15c4b-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15c4b-113">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="15c4b-114">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="15c4b-115">Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.</span><span class="sxs-lookup"><span data-stu-id="15c4b-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="15c4b-116">Selecione **Biblioteca de Classes Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15c4b-117">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15c4b-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="15c4b-118">Na linha de comando, execute `dotnet new razorclasslib`.</span><span class="sxs-lookup"><span data-stu-id="15c4b-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="15c4b-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="15c4b-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="15c4b-120">Para obter mais informações, confira [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="15c4b-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="15c4b-121">Adicione arquivos Razor na RCL.</span><span class="sxs-lookup"><span data-stu-id="15c4b-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="15c4b-122">É recomendável inserir o conteúdo da RCL na pasta *Áreas*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="15c4b-123">Referenciando o conteúdo da Biblioteca de Classes Razor</span><span class="sxs-lookup"><span data-stu-id="15c4b-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="15c4b-124">A RCL pode ser referenciada por:</span><span class="sxs-lookup"><span data-stu-id="15c4b-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="15c4b-125">Pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="15c4b-125">NuGet package.</span></span> <span data-ttu-id="15c4b-126">Confira [Criando pacotes do NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) e [Criar e publicar um pacote do NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="15c4b-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="15c4b-127">*{ProjectName}.csproj*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="15c4b-128">Confira [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="15c4b-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="15c4b-129">Acesso a arquivos parciais na RCL</span><span class="sxs-lookup"><span data-stu-id="15c4b-129">Partial files access in the RCL</span></span>

<span data-ttu-id="15c4b-130">Para o conteúdo externo da RCL, o tempo de execução do ASP.NET Core não pesquisa arquivos parciais na RCL.</span><span class="sxs-lookup"><span data-stu-id="15c4b-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="15c4b-131">Por exemplo, no download do exemplo, a exibição parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* **não** pode ser referenciada em *WebApp1\Pages\About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="15c4b-132">No entanto, as páginas na RCL (*RazorUIClassLib/* **podem** acessar *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="15c4b-133">Passo a passo: Criar um projeto de biblioteca de classes Razor e usar um projeto de Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="15c4b-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="15c4b-134">Você pode baixar o [projeto completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) e testá-lo em vez de criá-lo.</span><span class="sxs-lookup"><span data-stu-id="15c4b-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="15c4b-135">O download de exemplo contém um código adicional e links que facilitam o teste do projeto.</span><span class="sxs-lookup"><span data-stu-id="15c4b-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="15c4b-136">Você pode deixar comentários [nesse problema do GitHub](https://github.com/aspnet/Docs/issues/6098) com suas opiniões sobre os exemplos de download em relação às instruções passo a passo.</span><span class="sxs-lookup"><span data-stu-id="15c4b-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="15c4b-137">Testar o aplicativo de download</span><span class="sxs-lookup"><span data-stu-id="15c4b-137">Test the download app</span></span>

<span data-ttu-id="15c4b-138">Se você não tiver baixado o aplicativo concluído e preferir criar o projeto do passo a passo, vá para a [próxima seção](#create-a-razor-class-library).</span><span class="sxs-lookup"><span data-stu-id="15c4b-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15c4b-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15c4b-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="15c4b-140">Abra o arquivo *.sln* no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15c4b-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="15c4b-141">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15c4b-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15c4b-142">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15c4b-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="15c4b-143">Em um prompt de comando no diretório *cli*, crie a RCL e o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="15c4b-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="15c4b-144">Acesse o diretório *WebApp1* e execute o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="15c4b-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="15c4b-145">Siga as instruções em [Testar WebApp1](#test)</span><span class="sxs-lookup"><span data-stu-id="15c4b-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="15c4b-146">Criar uma Biblioteca de Classes Razor</span><span class="sxs-lookup"><span data-stu-id="15c4b-146">Create a Razor Class Library</span></span>

<span data-ttu-id="15c4b-147">Nesta seção, uma RCL (Biblioteca de Classes Razor) é criada.</span><span class="sxs-lookup"><span data-stu-id="15c4b-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="15c4b-148">Arquivos Razor são adicionados à RCL.</span><span class="sxs-lookup"><span data-stu-id="15c4b-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15c4b-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15c4b-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="15c4b-150">Crie o projeto da RCL:</span><span class="sxs-lookup"><span data-stu-id="15c4b-150">Create the RCL project:</span></span>

* <span data-ttu-id="15c4b-151">No menu **Arquivo** do Visual Studio, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="15c4b-152">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="15c4b-153">Nomeie o aplicativo como **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="15c4b-154">Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.</span><span class="sxs-lookup"><span data-stu-id="15c4b-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="15c4b-155">Selecione **Biblioteca de Classes Razor** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="15c4b-156">Crie aplicativo Web Páginas Razor:</span><span class="sxs-lookup"><span data-stu-id="15c4b-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="15c4b-157">No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução > **Adicionar** >  **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="15c4b-158">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="15c4b-159">Nomeie o aplicativo como **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="15c4b-160">Verifique se o **ASP.NET Core 2.1** ou posterior está selecionado.</span><span class="sxs-lookup"><span data-stu-id="15c4b-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="15c4b-161">Selecione **Aplicativo Web** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="15c4b-162">Adicione pastas e arquivos Razor ao projeto.</span><span class="sxs-lookup"><span data-stu-id="15c4b-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="15c4b-163">Adicione um arquivo Razor de exibição parcial chamado *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="15c4b-164">Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="15c4b-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="15c4b-165">Copie o arquivo *_ViewStart.cshtml* do projeto WebApp1 para *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="15c4b-166">O arquivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) é necessário para usar o layout do projeto Páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="15c4b-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15c4b-167">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15c4b-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="15c4b-168">Na linha de comando, execute o seguinte:</span><span class="sxs-lookup"><span data-stu-id="15c4b-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="15c4b-169">Os comandos anteriores:</span><span class="sxs-lookup"><span data-stu-id="15c4b-169">The preceding commands:</span></span>

* <span data-ttu-id="15c4b-170">Criam a RCL (Biblioteca de Classes Razor) `RazorUIClassLib`.</span><span class="sxs-lookup"><span data-stu-id="15c4b-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="15c4b-171">Criam uma página Razor _Message e a adicionam à RCL.</span><span class="sxs-lookup"><span data-stu-id="15c4b-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="15c4b-172">O parâmetro `-np` cria a página sem um `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="15c4b-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="15c4b-173">Criam um arquivo [viewstart](xref:mvc/views/layout#running-code-before-each-view) e o adicionam à RCL.</span><span class="sxs-lookup"><span data-stu-id="15c4b-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="15c4b-174">O arquivo viewstart é necessário para usar o layout do projeto Páginas Razor (que é adicionado na próxima seção).</span><span class="sxs-lookup"><span data-stu-id="15c4b-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="15c4b-175">Atualize as Páginas Razor:</span><span class="sxs-lookup"><span data-stu-id="15c4b-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="15c4b-176">Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="15c4b-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="15c4b-177">Substitua a marcação em *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="15c4b-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="15c4b-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` é necessário para usar a exibição parcial (`<partial name="_Message" />`).</span><span class="sxs-lookup"><span data-stu-id="15c4b-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="15c4b-179">Em vez de incluir a diretiva `@addTagHelper`, você pode adicionar um arquivo *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="15c4b-180">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="15c4b-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="15c4b-181">Para obter mais informações sobre viewimports, confira [Importando diretivas compartilhadas](xref:mvc/views/layout#importing-shared-directives)</span><span class="sxs-lookup"><span data-stu-id="15c4b-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="15c4b-182">Crie a biblioteca de classes para verificar se não há nenhum erro de compilador:</span><span class="sxs-lookup"><span data-stu-id="15c4b-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="15c4b-183">A saída do build contém *RazorUIClassLib.dll* e *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="15c4b-184">*RazorUIClassLib.Views.dll* contém o conteúdo Razor compilado.</span><span class="sxs-lookup"><span data-stu-id="15c4b-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="15c4b-185">Use a biblioteca da interface do usuário do Razor de um projeto Páginas Razor</span><span class="sxs-lookup"><span data-stu-id="15c4b-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15c4b-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15c4b-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15c4b-187">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Definir como projeto de inicialização**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="15c4b-188">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Dependências de Build** > **Dependências do Projeto**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="15c4b-189">Marque **RazorUIClassLib** como uma dependência de **WebApp1**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="15c4b-190">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **WebApp1** e selecione **Adicionar** > **Referência**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="15c4b-191">Na caixa de diálogo **Gerenciador de Referências**, marque **RazorUIClassLib** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="15c4b-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="15c4b-192">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="15c4b-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="15c4b-193">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="15c4b-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="15c4b-194">Crie um aplicativo Web Páginas Razor e um arquivo de solução contendo o aplicativo Páginas Razor e a Biblioteca de Classes Razor:</span><span class="sxs-lookup"><span data-stu-id="15c4b-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="15c4b-195">Crie e execute o aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="15c4b-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="15c4b-196">Testar o WebApp1</span><span class="sxs-lookup"><span data-stu-id="15c4b-196">Test WebApp1</span></span>

<span data-ttu-id="15c4b-197">Verifique se a biblioteca de classes da interface do usuário do Razor está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="15c4b-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="15c4b-198">Navegue para `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="15c4b-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="15c4b-199">Substituir exibições, exibições parciais e páginas</span><span class="sxs-lookup"><span data-stu-id="15c4b-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="15c4b-200">Quando uma exibição, uma exibição parcial ou uma Página Razor for encontrada no aplicativo Web e na Biblioteca de Classes Razor, a marcação Razor (arquivo *.cshtml*) no aplicativo Web terá precedência.</span><span class="sxs-lookup"><span data-stu-id="15c4b-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="15c4b-201">Por exemplo, adicione *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* ao WebApp1, e a Page1 ao WebApp1 terá precedência sobre a Page1 na Biblioteca de Classes do Razor.</span><span class="sxs-lookup"><span data-stu-id="15c4b-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="15c4b-202">No download de exemplo, renomeie *WebApp1/Areas/MyFeature2* como *WebApp1/Areas/MyFeature* para testar a precedência.</span><span class="sxs-lookup"><span data-stu-id="15c4b-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="15c4b-203">Copie a exibição parcial *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* para *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="15c4b-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="15c4b-204">Atualize a marcação para indicar o novo local.</span><span class="sxs-lookup"><span data-stu-id="15c4b-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="15c4b-205">Crie e execute o aplicativo para verificar se a versão da parcial do aplicativo está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="15c4b-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
