---
title: Migrando do ASP.NET MVC para o ASP.NET Core MVC
author: ardalis
description: "Saiba como começar a migração de um projeto ASP.NET MVC ao MVC do ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: c9c9f63cd635f364d9b2e081dc051a46a44d3e4f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="eef6c-103">Migrando do ASP.NET MVC para o ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="eef6c-103">Migrating from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="eef6c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="eef6c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="eef6c-105">Este artigo mostra como começar a migração de um projeto ASP.NET MVC para [MVC do ASP.NET Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="eef6c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="eef6c-106">O processo, ele destaca muitas coisas que foram alterados desde o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eef6c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="eef6c-107">Migrando do ASP.NET MVC é um processo de várias etapas e este artigo aborda a configuração inicial, controladores básico e modos de exibição, conteúdo estático e dependências do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="eef6c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="eef6c-108">Artigos adicionais abrangem migrando configuração e código de identidade encontrada em muitos projetos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eef6c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="eef6c-109">Os números de versão nos exemplos podem não ser atuais.</span><span class="sxs-lookup"><span data-stu-id="eef6c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="eef6c-110">Talvez seja necessário atualizar seus projetos adequadamente.</span><span class="sxs-lookup"><span data-stu-id="eef6c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="eef6c-111">Criar o projeto ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="eef6c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="eef6c-112">Para demonstrar a atualização, vamos começar criando um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eef6c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="eef6c-113">Criá-lo com o nome *WebApp1* para o namespace corresponderá o projeto do ASP.NET Core que criamos na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="eef6c-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Caixa de diálogo do Visual Studio novo projeto](mvc/_static/new-project.png)

![Caixa de diálogo nova aplicativo Web: modelo de projeto MVC selecionado no painel de modelos do ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="eef6c-116">*Opcional:* alterar o nome da solução de *WebApp1* para *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="eef6c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="eef6c-117">O Visual Studio exibirá o nome da nova solução (*Mvc5*), tornando mais fácil informar esse projeto da próximo projeto.</span><span class="sxs-lookup"><span data-stu-id="eef6c-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="eef6c-118">Criar o projeto do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eef6c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="eef6c-119">Criar um novo *vazio* aplicativo web do ASP.NET Core com o mesmo nome que o projeto anterior (*WebApp1*) para corresponder os namespaces em dois projetos.</span><span class="sxs-lookup"><span data-stu-id="eef6c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="eef6c-120">Ter o mesmo namespace torna mais fácil copiar código entre os dois projetos.</span><span class="sxs-lookup"><span data-stu-id="eef6c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="eef6c-121">Você precisará criar este projeto em um diretório diferente do projeto anterior para usar o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="eef6c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Caixa de diálogo Novo Projeto](mvc/_static/new_core.png)

![Caixa de diálogo nova aplicativo Web do ASP.NET: modelo de projeto vazio selecionado no painel de modelos do ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="eef6c-124">*Opcional:* criar um aplicativo ASP.NET Core novo usando o *aplicativo Web* modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="eef6c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="eef6c-125">Nomeie o projeto *WebApp1*e selecione uma opção de autenticação de **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="eef6c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="eef6c-126">Renomear este aplicativo para *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="eef6c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="eef6c-127">Criar este projeto economizará tempo na conversão.</span><span class="sxs-lookup"><span data-stu-id="eef6c-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="eef6c-128">Você pode examinar o código gerado pelo modelo para ver o resultado final ou copiar o código para o projeto de conversão.</span><span class="sxs-lookup"><span data-stu-id="eef6c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="eef6c-129">Também é útil quando você tiver problemas em uma etapa de conversão para comparar com o projeto de modelo gerado.</span><span class="sxs-lookup"><span data-stu-id="eef6c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="eef6c-130">Configurar o site para usar o MVC</span><span class="sxs-lookup"><span data-stu-id="eef6c-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="eef6c-131">Instalar o `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles` pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="eef6c-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="eef6c-132">`Microsoft.AspNetCore.Mvc` é a estrutura MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="eef6c-133">`Microsoft.AspNetCore.StaticFiles` é o manipulador de arquivo estático.</span><span class="sxs-lookup"><span data-stu-id="eef6c-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="eef6c-134">O tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos (consulte [trabalhando com arquivos estáticos](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="eef6c-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="eef6c-135">Abra o *. csproj* arquivo (com o botão direito no projeto no **Solution Explorer** e selecione **Editar WebApp1.csproj**) e adicione um `PrepareForPublish` destino:</span><span class="sxs-lookup"><span data-stu-id="eef6c-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="eef6c-136">O `PrepareForPublish` destino é necessária para adquirir as bibliotecas de cliente via Bower.</span><span class="sxs-lookup"><span data-stu-id="eef6c-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="eef6c-137">Falaremos sobre isso mais tarde.</span><span class="sxs-lookup"><span data-stu-id="eef6c-137">We'll talk about that later.</span></span>

* <span data-ttu-id="eef6c-138">Abra o *Startup.cs* de arquivo e altere o código para coincidir com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eef6c-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="eef6c-139">O `UseStaticFiles` método de extensão adiciona o manipulador de arquivo estático.</span><span class="sxs-lookup"><span data-stu-id="eef6c-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="eef6c-140">Conforme mencionado anteriormente, o tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="eef6c-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="eef6c-141">O `UseMvc` método de extensão adiciona o roteamento.</span><span class="sxs-lookup"><span data-stu-id="eef6c-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="eef6c-142">Para obter mais informações, consulte [inicialização do aplicativo](../fundamentals/startup.md) e [roteamento](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="eef6c-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="eef6c-143">Adicionar um controlador e o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="eef6c-143">Add a controller and view</span></span>

<span data-ttu-id="eef6c-144">Nesta seção, você adicionará um controlador mínimo e o modo de exibição para servir como espaços reservados para o controlador do ASP.NET MVC e modos de exibição que você vai migrar na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="eef6c-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="eef6c-145">Adicionar um *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="eef6c-146">Adicionar uma **classe do controlador MVC** com o nome *HomeController* para o *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Caixa de diálogo Adicionar Novo Item](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="eef6c-148">Adicionar um *exibições* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="eef6c-149">Adicionar um *exibições/inicial* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="eef6c-150">Adicionar uma *cshtml* página de exibição do MVC para o *exibições/inicial* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Caixa de diálogo Adicionar Novo Item](mvc/_static/view.png)

<span data-ttu-id="eef6c-152">A estrutura do projeto é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="eef6c-152">The project structure is shown below:</span></span>

![Gerenciador de soluções mostrando arquivos e pastas de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="eef6c-154">Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eef6c-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="eef6c-155">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="eef6c-155">Run the app.</span></span>

![Aplicativo Web aberto no Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="eef6c-157">Consulte [controladores](xref:mvc/controllers/actions) e [exibições](xref:mvc/views/overview) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="eef6c-157">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="eef6c-158">Agora que temos um projeto do ASP.NET Core trabalho mínimo, podemos começar a migrar a funcionalidade do projeto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eef6c-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="eef6c-159">Será necessário mover o seguinte:</span><span class="sxs-lookup"><span data-stu-id="eef6c-159">We will need to move the following:</span></span>

* <span data-ttu-id="eef6c-160">conteúdo do lado do cliente (CSS, fontes e scripts)</span><span class="sxs-lookup"><span data-stu-id="eef6c-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="eef6c-161">controladores</span><span class="sxs-lookup"><span data-stu-id="eef6c-161">controllers</span></span>

* <span data-ttu-id="eef6c-162">modos de exibição</span><span class="sxs-lookup"><span data-stu-id="eef6c-162">views</span></span>

* <span data-ttu-id="eef6c-163">modelos</span><span class="sxs-lookup"><span data-stu-id="eef6c-163">models</span></span>

* <span data-ttu-id="eef6c-164">Agrupamento</span><span class="sxs-lookup"><span data-stu-id="eef6c-164">bundling</span></span>

* <span data-ttu-id="eef6c-165">filtros</span><span class="sxs-lookup"><span data-stu-id="eef6c-165">filters</span></span>

* <span data-ttu-id="eef6c-166">Log de entrada/saída, de identidade (Isso será feito o seguinte tutorial.)</span><span class="sxs-lookup"><span data-stu-id="eef6c-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="eef6c-167">Controladores e exibições</span><span class="sxs-lookup"><span data-stu-id="eef6c-167">Controllers and views</span></span>

* <span data-ttu-id="eef6c-168">Copiar cada um dos métodos do ASP.NET MVC `HomeController` para o novo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="eef6c-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="eef6c-169">Observe que, no ASP.NET MVC, tipo de método de ação retorno controlador do modelo interno é [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); no ASP.NET MVC de núcleo, os métodos de ação retorno `IActionResult` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="eef6c-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="eef6c-170">`ActionResult` implementa `IActionResult`, portanto, não é necessário alterar o tipo de retorno de métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="eef6c-170">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="eef6c-171">Copie o *About.cshtml*, *Contact.cshtml*, e *cshtml* arquivos de exibição Razor do projeto ASP.NET MVC para o projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="eef6c-172">Executar o aplicativo do ASP.NET Core e cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="eef6c-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="eef6c-173">Ainda não migramos o arquivo de layout ou estilos ainda, para que os modos de exibição renderizados conterá apenas o conteúdo nos arquivos de exibição.</span><span class="sxs-lookup"><span data-stu-id="eef6c-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="eef6c-174">Você não terá os links dos arquivos gerados layout para o `About` e `Contact` exibe, para que você precisará invocá-los a partir do navegador (substitua **4492** com o número da porta usado no projeto).</span><span class="sxs-lookup"><span data-stu-id="eef6c-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contato](mvc/_static/contact-page.png)

<span data-ttu-id="eef6c-176">Observe que a falta de estilo e itens de menu.</span><span class="sxs-lookup"><span data-stu-id="eef6c-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="eef6c-177">Corrigiremos isso na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="eef6c-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="eef6c-178">Conteúdo estático</span><span class="sxs-lookup"><span data-stu-id="eef6c-178">Static content</span></span>

<span data-ttu-id="eef6c-179">Em versões anteriores do ASP.NET MVC, conteúdo estático hospedado da raiz do projeto da web e foi misturado com os arquivos do servidor.</span><span class="sxs-lookup"><span data-stu-id="eef6c-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="eef6c-180">No núcleo do ASP.NET, conteúdo estático é hospedado no *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="eef6c-181">Você desejará copiar o conteúdo estático de seu aplicativo ASP.NET MVC antigo para o *wwwroot* pasta em seu projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="eef6c-182">Nessa conversão de exemplo:</span><span class="sxs-lookup"><span data-stu-id="eef6c-182">In this sample conversion:</span></span>

* <span data-ttu-id="eef6c-183">Copiar o *favicon.ico* arquivo de projeto MVC antigo para o *wwwroot* pasta do projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="eef6c-184">O ASP.NET MVC antigo projeto usa [Bootstrap](http://getbootstrap.com/) para seu estilo e repositórios de arquivos a inicialização a *conteúdo* e *Scripts* pastas.</span><span class="sxs-lookup"><span data-stu-id="eef6c-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="eef6c-185">O modelo, que gerou o antigo projeto ASP.NET MVC, referencia o Bootstrap no arquivo de layout (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="eef6c-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="eef6c-186">Você poderá copiar o *bootstrap.js* e *bootstrap.css* projeto de arquivos do ASP.NET MVC para o *wwwroot* pasta no novo projeto, mas essa abordagem não usa o melhor mecanismo para gerenciar o cliente dependências no núcleo do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eef6c-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="eef6c-187">No novo projeto, vamos adicionar suporte para inicialização (e outras bibliotecas de cliente) usando [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="eef6c-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="eef6c-188">Adicionar um [Bower](https://bower.io/) arquivo de configuração chamado *bower. JSON* para a raiz do projeto (com o botão direito no projeto e, em seguida, **Adicionar > Novo Item > arquivo de configuração Bower**).</span><span class="sxs-lookup"><span data-stu-id="eef6c-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="eef6c-189">Adicionar [Bootstrap](http://getbootstrap.com/) e [jQuery](https://jquery.com/) para o arquivo (consulte as linhas destacadas abaixo).</span><span class="sxs-lookup"><span data-stu-id="eef6c-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="eef6c-190">Após salvar o arquivo, o Bower baixará automaticamente as dependências para o *wwwroot/lib* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="eef6c-191">Você pode usar o **pesquisar Gerenciador de soluções** para localizar o caminho dos ativos:</span><span class="sxs-lookup"><span data-stu-id="eef6c-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![jQuery ativos mostrados nos resultados da pesquisa do Gerenciador de soluções](mvc/_static/search.png)

<span data-ttu-id="eef6c-193">Consulte [gerenciar pacotes do lado do cliente com Bower](../client-side/bower.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="eef6c-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="eef6c-194">Migrar o arquivo de layout</span><span class="sxs-lookup"><span data-stu-id="eef6c-194">Migrate the layout file</span></span>

* <span data-ttu-id="eef6c-195">Copiar o *viewstart* arquivo a partir do projeto ASP.NET MVC antigo *exibições* pasta para do projeto ASP.NET Core *exibições* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="eef6c-196">O *viewstart* arquivo não foi alterado no MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="eef6c-197">Criar um *exibições/compartilhadas* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="eef6c-198">*Opcional:* cópia *viewimports. cshtml* do *FullAspNetCore* do projeto MVC *exibições* pasta para do projeto ASP.NET Core  *Modos de exibição* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="eef6c-199">Remover qualquer declaração de namespace no *viewimports. cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="eef6c-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="eef6c-200">O *viewimports. cshtml* arquivo fornece namespaces para todos os arquivos de exibição e coloca [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="eef6c-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="eef6c-201">Os auxiliares de marca são usados no novo arquivo de layout.</span><span class="sxs-lookup"><span data-stu-id="eef6c-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="eef6c-202">O *viewimports. cshtml* arquivo é novo para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eef6c-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="eef6c-203">Copie o *cshtml* arquivo a partir do projeto ASP.NET MVC antigo *exibições/compartilhadas* pasta para do projeto ASP.NET Core *exibições/compartilhadas* pasta.</span><span class="sxs-lookup"><span data-stu-id="eef6c-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="eef6c-204">Abra *cshtml* de arquivo e faça as alterações a seguir (o código completo é mostrado abaixo):</span><span class="sxs-lookup"><span data-stu-id="eef6c-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="eef6c-205">Substituir `@Styles.Render("~/Content/css")` com um `<link>` elemento carregar *bootstrap.css* (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="eef6c-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="eef6c-206">Remova `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="eef6c-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="eef6c-207">Comente o `@Html.Partial("_LoginPartial")` linha (envolvem a linha com `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="eef6c-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="eef6c-208">Retornaremos a ele um tutorial futuras.</span><span class="sxs-lookup"><span data-stu-id="eef6c-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="eef6c-209">Substituir `@Scripts.Render("~/bundles/jquery")` com um `<script>` elemento (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="eef6c-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="eef6c-210">Substituir `@Scripts.Render("~/bundles/bootstrap")` com um `<script>` elemento (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="eef6c-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="eef6c-211">O link CSS de substituição:</span><span class="sxs-lookup"><span data-stu-id="eef6c-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="eef6c-212">As marcas de script de substituição:</span><span class="sxs-lookup"><span data-stu-id="eef6c-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="eef6c-213">A atualização *cshtml* arquivo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="eef6c-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="eef6c-214">Exiba o site no navegador.</span><span class="sxs-lookup"><span data-stu-id="eef6c-214">View the site in the browser.</span></span> <span data-ttu-id="eef6c-215">Ele agora deve carregar corretamente, com os estilos esperados em vigor.</span><span class="sxs-lookup"><span data-stu-id="eef6c-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="eef6c-216">*Opcional:* você talvez queira usar o novo arquivo de layout.</span><span class="sxs-lookup"><span data-stu-id="eef6c-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="eef6c-217">Para este projeto, você pode copiar o arquivo de layout do *FullAspNetCore* projeto.</span><span class="sxs-lookup"><span data-stu-id="eef6c-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="eef6c-218">O novo arquivo de layout usa [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) e tiver outros aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="eef6c-218">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="eef6c-219">Configurar o empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="eef6c-219">Configure bundling and minification</span></span>

<span data-ttu-id="eef6c-220">Para obter informações sobre como configurar o empacotamento e minimização, consulte [empacotamento e minimização](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="eef6c-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="eef6c-221">Resolver erros HTTP 500</span><span class="sxs-lookup"><span data-stu-id="eef6c-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="eef6c-222">Há muitos problemas que podem causar uma mensagem de erro HTTP 500 que não contêm informações sobre a origem do problema.</span><span class="sxs-lookup"><span data-stu-id="eef6c-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="eef6c-223">Por exemplo, se o *Views/_ViewImports.cshtml* arquivo contém um namespace que não existe em seu projeto, você obterá um erro HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="eef6c-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="eef6c-224">Para obter uma mensagem de erro detalhada, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="eef6c-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="eef6c-225">Consulte **usando a página de exceção de desenvolvedor** na [tratamento de erros](../fundamentals/error-handling.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="eef6c-225">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eef6c-226">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="eef6c-226">Additional resources</span></span>

* [<span data-ttu-id="eef6c-227">Desenvolvimento no Lado do Cliente</span><span class="sxs-lookup"><span data-stu-id="eef6c-227">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="eef6c-228">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="eef6c-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
