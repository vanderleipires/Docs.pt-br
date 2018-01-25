---
title: "Usando o Bower no núcleo do ASP.NET"
author: rick-anderson
description: Gerenciar pacotes do lado do cliente com Bower.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e3e936c81126b7ed01332565f997910a2886993
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="23aac-103">Gerenciar pacotes do lado do cliente com Bower no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="23aac-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="23aac-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="23aac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="23aac-105">Enquanto Bower é mantido, eles recomendam usar uma solução diferente.</span><span class="sxs-lookup"><span data-stu-id="23aac-105">While Bower is maintained, they recommend using a different solution.</span></span> <span data-ttu-id="23aac-106">Yarn com Webpack é uma alternativa popular para a qual [instruções de migração](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="23aac-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="23aac-107">[Bower](https://bower.io/) chama a mesmo "Um Gerenciador de pacotes para a web".</span><span class="sxs-lookup"><span data-stu-id="23aac-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="23aac-108">No ecossistema do .NET, ele preenche essa lacuna deixado pela incapacidade do NuGet para entregar os arquivos de conteúdo estáticos.</span><span class="sxs-lookup"><span data-stu-id="23aac-108">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="23aac-109">Para projetos do ASP.NET Core, esses arquivos estáticos são inerentes a bibliotecas de cliente como [jQuery](http://jquery.com/) e [inicialização](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="23aac-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="23aac-110">Para bibliotecas .NET, você usar [NuGet](https://www.nuget.org/) Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="23aac-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="23aac-111">Processo de compilação de projetos novos criados com os modelos de projeto do ASP.NET Core configurar lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="23aac-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="23aac-112">[jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/) são instalados, e tem suporte em Bower.</span><span class="sxs-lookup"><span data-stu-id="23aac-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="23aac-113">Pacotes do lado do cliente são listados no *bower. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="23aac-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="23aac-114">Configura os modelos de projeto do ASP.NET Core *bower. JSON* com inicialização, validação jQuery e jQuery.</span><span class="sxs-lookup"><span data-stu-id="23aac-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="23aac-115">Neste tutorial, vamos adicionar suporte para [fonte Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="23aac-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="23aac-116">Bower pacotes podem ser instalados com o **gerenciar pacotes de Bower** interface do usuário ou manualmente o *bower. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="23aac-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="23aac-117">Instalação por meio de gerenciar Bower pacotes da interface do usuário</span><span class="sxs-lookup"><span data-stu-id="23aac-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="23aac-118">Criar um novo aplicativo Web do ASP.NET Core com o **aplicativo Web do ASP.NET Core (.NET Core)** modelo.</span><span class="sxs-lookup"><span data-stu-id="23aac-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="23aac-119">Selecione **aplicativo Web** e **nenhuma autenticação**.</span><span class="sxs-lookup"><span data-stu-id="23aac-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="23aac-120">Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar pacotes de Bower** (como alternativa no menu principal, **projeto** > **gerenciar pacotes Bower**).</span><span class="sxs-lookup"><span data-stu-id="23aac-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="23aac-121">No **Bower: \<nome do projeto\>**  janela, clique na guia de "Procurar" e, em seguida, filtre a lista de pacotes inserindo `font-awesome` na caixa de pesquisa:</span><span class="sxs-lookup"><span data-stu-id="23aac-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![Gerenciar pacotes bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="23aac-123">Confirme se o "Salvar alterações em *bower. JSON*" caixa de seleção é marcada.</span><span class="sxs-lookup"><span data-stu-id="23aac-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="23aac-124">Selecione uma versão na lista suspensa e clique no **instalar** botão.</span><span class="sxs-lookup"><span data-stu-id="23aac-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="23aac-125">O **saída** janela mostra os detalhes da instalação.</span><span class="sxs-lookup"><span data-stu-id="23aac-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="23aac-126">Instalação manual em bower. JSON</span><span class="sxs-lookup"><span data-stu-id="23aac-126">Manual installation in bower.json</span></span>

<span data-ttu-id="23aac-127">Abra o *bower. JSON* e adicione "fonte o incríveis" para as dependências.</span><span class="sxs-lookup"><span data-stu-id="23aac-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="23aac-128">IntelliSense mostra os pacotes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="23aac-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="23aac-129">Quando um pacote é selecionado, as versões disponíveis são exibidas.</span><span class="sxs-lookup"><span data-stu-id="23aac-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="23aac-130">As imagens abaixo são mais antigas e não coincidir com o que você vê.</span><span class="sxs-lookup"><span data-stu-id="23aac-130">The images below are older and won't match what you see.</span></span>

![IntelliSense do Explorador de pacotes bower](bower/_static/add-package.png)

![versão bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="23aac-133">Bower usa [controle de versão semântico](http://semver.org/) para organizar as dependências.</span><span class="sxs-lookup"><span data-stu-id="23aac-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="23aac-134">Controle de versão semântico, também conhecido como SemVer, identifica os pacotes com o esquema de numeração \<principal >.\< secundária >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="23aac-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="23aac-135">IntelliSense simplifica o controle de versão semântico, mostrando apenas algumas opções comuns.</span><span class="sxs-lookup"><span data-stu-id="23aac-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="23aac-136">O item superior na lista do IntelliSense (4.6.3 no exemplo acima) é considerado a versão estável mais recente do pacote.</span><span class="sxs-lookup"><span data-stu-id="23aac-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="23aac-137">O símbolo de acento circunflexo (^) corresponde a mais recente versão principal e o til (~) corresponde a versão secundária mais recente.</span><span class="sxs-lookup"><span data-stu-id="23aac-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="23aac-138">Salve o *bower. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="23aac-138">Save the *bower.json* file.</span></span> <span data-ttu-id="23aac-139">O Visual Studio inspeciona o *bower. JSON* arquivo para que as alterações.</span><span class="sxs-lookup"><span data-stu-id="23aac-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="23aac-140">Ao salvar, o *bower instalar* comando é executado.</span><span class="sxs-lookup"><span data-stu-id="23aac-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="23aac-141">Consulte a janela de saída **npm/Bower** modo de exibição para o comando executado.</span><span class="sxs-lookup"><span data-stu-id="23aac-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="23aac-142">Abra o *bowerrc* de arquivos em *bower. JSON*.</span><span class="sxs-lookup"><span data-stu-id="23aac-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="23aac-143">O `directory` está definida como *wwwroot/lib* que indica o local Bower instalará os ativos de pacote.</span><span class="sxs-lookup"><span data-stu-id="23aac-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="23aac-144">Você pode usar a caixa de pesquisa no Gerenciador de soluções para localizar e exibir o pacote incrível de fonte.</span><span class="sxs-lookup"><span data-stu-id="23aac-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="23aac-145">Abra o *exibições \ compartilhadas\_cshtml* de arquivo e adicione o arquivo CSS incrível de fonte para o ambiente [auxiliar de marca](xref:mvc/views/tag-helpers/intro) para `Development`.</span><span class="sxs-lookup"><span data-stu-id="23aac-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="23aac-146">No Gerenciador de soluções, arrastar e soltar *fonte awesome.css* dentro de `<environment names="Development">` elemento.</span><span class="sxs-lookup"><span data-stu-id="23aac-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="23aac-147">Um aplicativo de produção, você adicionaria *fonte awesome.min.css* para o auxiliar de marca de ambiente para `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="23aac-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="23aac-148">Substitua o conteúdo do *Views\Home\About.cshtml* arquivo Razor com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="23aac-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="23aac-149">Execute o aplicativo e navegue até o modo de exibição sobre para verificar o funcionamento do pacote incrível de fonte.</span><span class="sxs-lookup"><span data-stu-id="23aac-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="23aac-150">Explorando o processo de compilação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="23aac-150">Exploring the client-side build process</span></span>

<span data-ttu-id="23aac-151">A maioria dos modelos de projeto do ASP.NET Core já estão configurados para usar o Bower.</span><span class="sxs-lookup"><span data-stu-id="23aac-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="23aac-152">Este passo a passo próxima começa com um projeto vazio do ASP.NET Core e adiciona cada pedaço manualmente, portanto você pode ter uma ideia de como Bower é usado em um projeto.</span><span class="sxs-lookup"><span data-stu-id="23aac-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="23aac-153">Você pode ver o que acontece com a estrutura do projeto e o tempo de execução de saída como cada alteração de configuração é feita.</span><span class="sxs-lookup"><span data-stu-id="23aac-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="23aac-154">As etapas gerais para usar o processo de compilação do lado do cliente com Bower são:</span><span class="sxs-lookup"><span data-stu-id="23aac-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="23aac-155">Defina pacotes usados em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="23aac-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="23aac-156">Pacotes de referência de suas páginas da web.</span><span class="sxs-lookup"><span data-stu-id="23aac-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="23aac-157">Definir pacotes</span><span class="sxs-lookup"><span data-stu-id="23aac-157">Define packages</span></span>

<span data-ttu-id="23aac-158">Depois que você listar pacotes no *bower. JSON* arquivo, o Visual Studio irá baixá-los.</span><span class="sxs-lookup"><span data-stu-id="23aac-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="23aac-159">O exemplo a seguir usa Bower para carregar jQuery e inicialização para o *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="23aac-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="23aac-160">Criar um novo aplicativo Web do ASP.NET Core com o **aplicativo Web do ASP.NET Core (.NET Core)** modelo.</span><span class="sxs-lookup"><span data-stu-id="23aac-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="23aac-161">Selecione o **vazio** modelo de projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="23aac-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="23aac-162">No Gerenciador de soluções, clique com botão direito no projeto > **Adicionar Novo Item** e selecione **Bower arquivo de configuração**.</span><span class="sxs-lookup"><span data-stu-id="23aac-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="23aac-163">Observação: A *bowerrc* arquivo também é adicionado.</span><span class="sxs-lookup"><span data-stu-id="23aac-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="23aac-164">Abra *bower. JSON*, adicione jquery e inicialização para o `dependencies` seção.</span><span class="sxs-lookup"><span data-stu-id="23aac-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="23aac-165">Resultante *bower. JSON* arquivo se parecerá com o exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="23aac-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="23aac-166">As versões serão alterado ao longo do tempo e podem não corresponder a imagem a seguir.</span><span class="sxs-lookup"><span data-stu-id="23aac-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="23aac-167">Salve o *bower. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="23aac-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="23aac-168">Verifique se o projeto inclui o *bootstrap* e *jQuery* diretórios *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="23aac-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="23aac-169">Bower usa o *bowerrc* arquivo para instalar os ativos em *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="23aac-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="23aac-170">Observação: A interface do usuário "Gerenciar pacotes de Bower" fornece uma alternativa à edição de arquivos manual.</span><span class="sxs-lookup"><span data-stu-id="23aac-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="23aac-171">Habilitar arquivos estáticos</span><span class="sxs-lookup"><span data-stu-id="23aac-171">Enable static files</span></span>

* <span data-ttu-id="23aac-172">Adicionar o `Microsoft.AspNetCore.StaticFiles` pacote NuGet para o projeto.</span><span class="sxs-lookup"><span data-stu-id="23aac-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="23aac-173">Habilitar arquivos estáticos sejam atendidos com o [middleware de arquivo estático](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="23aac-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="23aac-174">Adicionar uma chamada para [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) para o `Configure` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="23aac-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="23aac-175">Pacotes de referência</span><span class="sxs-lookup"><span data-stu-id="23aac-175">Reference packages</span></span>

<span data-ttu-id="23aac-176">Nesta seção, você criará uma página HTML para verificar se que ele pode acessar os pacotes implantados.</span><span class="sxs-lookup"><span data-stu-id="23aac-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="23aac-177">Adicionar uma nova página HTML chamada *Index.html* para o *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="23aac-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="23aac-178">Observação: Você deve adicionar o arquivo HTML para o *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="23aac-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="23aac-179">Por padrão, o conteúdo estático não pode ser servido fora *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="23aac-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="23aac-180">Consulte [trabalhando com arquivos estáticos](xref:fundamentals/static-files) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="23aac-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="23aac-181">Substitua o conteúdo do *index* com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="23aac-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="23aac-182">Execute o aplicativo e navegue até `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="23aac-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="23aac-183">Como alternativa, com *Index.html* aberta, pressione `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="23aac-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="23aac-184">Verifique se que o estilo de jumbotron é aplicado, o código jQuery responde quando o botão é clicado e que o botão inicialização muda de estado.</span><span class="sxs-lookup"><span data-stu-id="23aac-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![estilo de jumbotron aplicado](bower/_static/jumbotron.png)
