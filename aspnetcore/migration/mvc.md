---
title: Migrar do ASP.NET MVC para o núcleo do ASP.NET MVC
author: ardalis
description: Saiba como começar a migração de um projeto ASP.NET MVC ao MVC do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851021"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="7da4c-103">Migrar do ASP.NET MVC para o núcleo do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7da4c-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="7da4c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="7da4c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="7da4c-105">Este artigo mostra como começar a migração de um projeto ASP.NET MVC para [MVC do ASP.NET Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="7da4c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="7da4c-106">O processo, ele destaca muitas coisas que foram alterados desde o ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7da4c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="7da4c-107">Migrando do ASP.NET MVC é um processo de várias etapas e este artigo aborda a configuração inicial, controladores básico e modos de exibição, conteúdo estático e dependências do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="7da4c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="7da4c-108">Artigos adicionais abrangem migrando configuração e código de identidade encontrada em muitos projetos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7da4c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="7da4c-109">Os números de versão nos exemplos podem não ser atuais.</span><span class="sxs-lookup"><span data-stu-id="7da4c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="7da4c-110">Talvez seja necessário atualizar seus projetos adequadamente.</span><span class="sxs-lookup"><span data-stu-id="7da4c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="7da4c-111">Criar o projeto ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="7da4c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="7da4c-112">Para demonstrar a atualização, vamos começar criando um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7da4c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="7da4c-113">Criá-lo com o nome *WebApp1* para o espaço para nome coincida com o projeto do ASP.NET Core que criamos na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="7da4c-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Caixa de diálogo do Visual Studio novo projeto](mvc/_static/new-project.png)

![Caixa de diálogo nova aplicativo Web: modelo de projeto MVC selecionado no painel de modelos do ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="7da4c-116">*Opcional:* alterar o nome da solução de *WebApp1* para *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="7da4c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="7da4c-117">O Visual Studio exibe o novo nome de solução (*Mvc5*), que torna mais fácil informar esse projeto da próximo projeto.</span><span class="sxs-lookup"><span data-stu-id="7da4c-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="7da4c-118">Criar o projeto do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7da4c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="7da4c-119">Criar um novo *vazio* aplicativo web do ASP.NET Core com o mesmo nome que o projeto anterior (*WebApp1*) para corresponder os namespaces em dois projetos.</span><span class="sxs-lookup"><span data-stu-id="7da4c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="7da4c-120">Ter o mesmo namespace torna mais fácil copiar código entre os dois projetos.</span><span class="sxs-lookup"><span data-stu-id="7da4c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="7da4c-121">Você precisará criar este projeto em um diretório diferente do projeto anterior para usar o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="7da4c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Caixa de diálogo Novo Projeto](mvc/_static/new_core.png)

![Caixa de diálogo nova aplicativo Web do ASP.NET: modelo de projeto vazio selecionado no painel de modelos do ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="7da4c-124">*Opcional:* criar um aplicativo ASP.NET Core novo usando o *aplicativo Web* modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="7da4c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="7da4c-125">Nomeie o projeto *WebApp1*e selecione uma opção de autenticação de **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="7da4c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="7da4c-126">Renomear este aplicativo para *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="7da4c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="7da4c-127">Criando isso economiza projeto tempo na conversão.</span><span class="sxs-lookup"><span data-stu-id="7da4c-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="7da4c-128">Você pode examinar o código gerado pelo modelo para ver o resultado final ou copiar o código para o projeto de conversão.</span><span class="sxs-lookup"><span data-stu-id="7da4c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="7da4c-129">Também é útil quando você tiver problemas em uma etapa de conversão para comparar com o projeto de modelo gerado.</span><span class="sxs-lookup"><span data-stu-id="7da4c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="7da4c-130">Configurar o site para usar o MVC</span><span class="sxs-lookup"><span data-stu-id="7da4c-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="7da4c-131">Durante o direcionamento o núcleo do .NET, o ASP.NET Core metapackage é adicionado ao projeto, chamado `Microsoft.AspNetCore.All` por padrão.</span><span class="sxs-lookup"><span data-stu-id="7da4c-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="7da4c-132">Este pacote contém os pacotes como `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="7da4c-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="7da4c-133">Se o destino do .NET Framework, referências de pacote precisam listada individualmente no arquivo csproj.</span><span class="sxs-lookup"><span data-stu-id="7da4c-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="7da4c-134">`Microsoft.AspNetCore.Mvc` é a estrutura MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="7da4c-135">`Microsoft.AspNetCore.StaticFiles` é o manipulador de arquivo estático.</span><span class="sxs-lookup"><span data-stu-id="7da4c-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="7da4c-136">O tempo de execução do ASP.NET Core é modular, e você deve optar explicitamente para servir arquivos estáticos (consulte [arquivos estáticos](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="7da4c-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="7da4c-137">Abra o *Startup.cs* de arquivo e altere o código para coincidir com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7da4c-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="7da4c-138">O `UseStaticFiles` método de extensão adiciona o manipulador de arquivo estático.</span><span class="sxs-lookup"><span data-stu-id="7da4c-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="7da4c-139">Conforme mencionado anteriormente, o tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7da4c-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="7da4c-140">O `UseMvc` método de extensão adiciona o roteamento.</span><span class="sxs-lookup"><span data-stu-id="7da4c-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="7da4c-141">Para obter mais informações, consulte [inicialização do aplicativo](xref:fundamentals/startup) e [roteamento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="7da4c-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="7da4c-142">Adicionar um controlador e o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="7da4c-142">Add a controller and view</span></span>

<span data-ttu-id="7da4c-143">Nesta seção, você adicionará um controlador mínimo e o modo de exibição para servir como espaços reservados para o controlador do ASP.NET MVC e modos de exibição que você vai migrar na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="7da4c-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="7da4c-144">Adicionar um *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="7da4c-145">Adicionar um **classe Controller** chamado *HomeController* para o *controladores* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Caixa de diálogo Adicionar Novo Item](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="7da4c-147">Adicionar um *exibições* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="7da4c-148">Adicionar um *exibições/inicial* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="7da4c-149">Adicionar um **exibição Razor** chamado *cshtml* para o *exibições/inicial* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Caixa de diálogo Adicionar Novo Item](mvc/_static/view.png)

<span data-ttu-id="7da4c-151">A estrutura do projeto é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="7da4c-151">The project structure is shown below:</span></span>

![Gerenciador de soluções mostrando arquivos e pastas de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="7da4c-153">Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7da4c-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="7da4c-154">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7da4c-154">Run the app.</span></span>

![Aplicativo Web aberto no Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="7da4c-156">Consulte [controladores](xref:mvc/controllers/actions) e [exibições](xref:mvc/views/overview) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7da4c-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="7da4c-157">Agora que temos um projeto do ASP.NET Core trabalho mínimo, podemos começar a migrar a funcionalidade do projeto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7da4c-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="7da4c-158">É necessário mover o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7da4c-158">We need to move the following:</span></span>

* <span data-ttu-id="7da4c-159">conteúdo do lado do cliente (CSS, fontes e scripts)</span><span class="sxs-lookup"><span data-stu-id="7da4c-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="7da4c-160">controladores</span><span class="sxs-lookup"><span data-stu-id="7da4c-160">controllers</span></span>

* <span data-ttu-id="7da4c-161">modos de exibição</span><span class="sxs-lookup"><span data-stu-id="7da4c-161">views</span></span>

* <span data-ttu-id="7da4c-162">modelos</span><span class="sxs-lookup"><span data-stu-id="7da4c-162">models</span></span>

* <span data-ttu-id="7da4c-163">Agrupamento</span><span class="sxs-lookup"><span data-stu-id="7da4c-163">bundling</span></span>

* <span data-ttu-id="7da4c-164">filtros</span><span class="sxs-lookup"><span data-stu-id="7da4c-164">filters</span></span>

* <span data-ttu-id="7da4c-165">Log de entrada/saída, de identidade (Isso é feito no tutorial próximo).</span><span class="sxs-lookup"><span data-stu-id="7da4c-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="7da4c-166">Controladores e exibições</span><span class="sxs-lookup"><span data-stu-id="7da4c-166">Controllers and views</span></span>

* <span data-ttu-id="7da4c-167">Copiar cada um dos métodos do ASP.NET MVC `HomeController` para o novo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="7da4c-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="7da4c-168">Observe que, no ASP.NET MVC, tipo de método de ação retorno controlador do modelo interno é [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); no ASP.NET MVC de núcleo, os métodos de ação retorno `IActionResult` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="7da4c-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="7da4c-169">`ActionResult` implementa `IActionResult`, portanto, não é necessário alterar o tipo de retorno de métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="7da4c-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="7da4c-170">Copie o *About.cshtml*, *Contact.cshtml*, e *cshtml* arquivos de exibição Razor do projeto ASP.NET MVC para o projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="7da4c-171">Executar o aplicativo do ASP.NET Core e cada método de teste.</span><span class="sxs-lookup"><span data-stu-id="7da4c-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="7da4c-172">Ainda não migramos o arquivo de layout ou estilos ainda, para que os modos de exibição renderizados só contêm o conteúdo de arquivos de exibição.</span><span class="sxs-lookup"><span data-stu-id="7da4c-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="7da4c-173">Você não terá os links dos arquivos gerados layout para o `About` e `Contact` exibe, para que você precisará invocá-los a partir do navegador (substitua **4492** com o número da porta usado no projeto).</span><span class="sxs-lookup"><span data-stu-id="7da4c-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contato](mvc/_static/contact-page.png)

<span data-ttu-id="7da4c-175">Observe que a falta de estilo e itens de menu.</span><span class="sxs-lookup"><span data-stu-id="7da4c-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="7da4c-176">Corrigiremos isso na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="7da4c-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="7da4c-177">Conteúdo estático</span><span class="sxs-lookup"><span data-stu-id="7da4c-177">Static content</span></span>

<span data-ttu-id="7da4c-178">Em versões anteriores do ASP.NET MVC, conteúdo estático hospedado da raiz do projeto da web e foi misturado com os arquivos do servidor.</span><span class="sxs-lookup"><span data-stu-id="7da4c-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="7da4c-179">No núcleo do ASP.NET, conteúdo estático é hospedado no *wwwroot* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="7da4c-180">Você desejará copiar o conteúdo estático de seu aplicativo ASP.NET MVC antigo para o *wwwroot* pasta em seu projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="7da4c-181">Nessa conversão de exemplo:</span><span class="sxs-lookup"><span data-stu-id="7da4c-181">In this sample conversion:</span></span>

* <span data-ttu-id="7da4c-182">Copiar o *favicon.ico* arquivo de projeto MVC antigo para o *wwwroot* pasta do projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="7da4c-183">O ASP.NET MVC antigo projeto usa [Bootstrap](https://getbootstrap.com/) para seu estilo e repositórios de arquivos a inicialização a *conteúdo* e *Scripts* pastas.</span><span class="sxs-lookup"><span data-stu-id="7da4c-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="7da4c-184">O modelo, que gerou o antigo projeto ASP.NET MVC, referencia o Bootstrap no arquivo de layout (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7da4c-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="7da4c-185">Você poderá copiar o *bootstrap.js* e *bootstrap.css* projeto de arquivos do ASP.NET MVC para o *wwwroot* pasta no novo projeto.</span><span class="sxs-lookup"><span data-stu-id="7da4c-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="7da4c-186">Em vez disso, vamos adicionar suporte para inicialização (e outras bibliotecas de cliente) usando CDNs na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="7da4c-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="7da4c-187">Migrar o arquivo de layout</span><span class="sxs-lookup"><span data-stu-id="7da4c-187">Migrate the layout file</span></span>

* <span data-ttu-id="7da4c-188">Copiar o *viewstart* arquivo a partir do projeto ASP.NET MVC antigo *exibições* pasta para do projeto ASP.NET Core *exibições* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="7da4c-189">O *viewstart* arquivo não foi alterado no MVC do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="7da4c-190">Criar um *exibições/compartilhadas* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="7da4c-191">*Opcional:* cópia *viewimports. cshtml* do *FullAspNetCore* do projeto MVC *exibições* pasta para do projeto ASP.NET Core  *Modos de exibição* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="7da4c-192">Remover qualquer declaração de namespace no *viewimports. cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7da4c-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="7da4c-193">O *viewimports. cshtml* arquivo fornece namespaces para todos os arquivos de exibição e coloca [auxiliares de marcação](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="7da4c-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="7da4c-194">Os auxiliares de marca são usados no novo arquivo de layout.</span><span class="sxs-lookup"><span data-stu-id="7da4c-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="7da4c-195">O *viewimports. cshtml* arquivo é novo para o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7da4c-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="7da4c-196">Copie o *cshtml* arquivo a partir do projeto ASP.NET MVC antigo *exibições/compartilhadas* pasta para do projeto ASP.NET Core *exibições/compartilhadas* pasta.</span><span class="sxs-lookup"><span data-stu-id="7da4c-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="7da4c-197">Abra *cshtml* de arquivo e faça as alterações a seguir (o código completo é mostrado abaixo):</span><span class="sxs-lookup"><span data-stu-id="7da4c-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="7da4c-198">Substituir `@Styles.Render("~/Content/css")` com um `<link>` elemento carregar *bootstrap.css* (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="7da4c-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="7da4c-199">Remova `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="7da4c-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="7da4c-200">Comente o `@Html.Partial("_LoginPartial")` linha (envolvem a linha com `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="7da4c-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="7da4c-201">Retornaremos a ele um tutorial futuras.</span><span class="sxs-lookup"><span data-stu-id="7da4c-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="7da4c-202">Substituir `@Scripts.Render("~/bundles/jquery")` com um `<script>` elemento (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="7da4c-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="7da4c-203">Substituir `@Scripts.Render("~/bundles/bootstrap")` com um `<script>` elemento (veja abaixo).</span><span class="sxs-lookup"><span data-stu-id="7da4c-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="7da4c-204">A marcação de substituição para inclusão de inicialização CSS:</span><span class="sxs-lookup"><span data-stu-id="7da4c-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="7da4c-205">A marcação de substituição para jQuery e inclusão de inicialização JavaScript:</span><span class="sxs-lookup"><span data-stu-id="7da4c-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="7da4c-206">A atualização *cshtml* arquivo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="7da4c-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="7da4c-207">Exiba o site no navegador.</span><span class="sxs-lookup"><span data-stu-id="7da4c-207">View the site in the browser.</span></span> <span data-ttu-id="7da4c-208">Ele agora deve carregar corretamente, com os estilos esperados em vigor.</span><span class="sxs-lookup"><span data-stu-id="7da4c-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="7da4c-209">*Opcional:* você talvez queira usar o novo arquivo de layout.</span><span class="sxs-lookup"><span data-stu-id="7da4c-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="7da4c-210">Para este projeto, você pode copiar o arquivo de layout do *FullAspNetCore* projeto.</span><span class="sxs-lookup"><span data-stu-id="7da4c-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="7da4c-211">O novo arquivo de layout usa [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) e tiver outros aprimoramentos.</span><span class="sxs-lookup"><span data-stu-id="7da4c-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="7da4c-212">Configurar o empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="7da4c-212">Configure bundling and minification</span></span>

<span data-ttu-id="7da4c-213">Para obter informações sobre como configurar o empacotamento e minimização, consulte [empacotamento e minimização](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="7da4c-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="7da4c-214">Resolver erros HTTP 500</span><span class="sxs-lookup"><span data-stu-id="7da4c-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="7da4c-215">Há muitos problemas que podem causar uma mensagem de erro HTTP 500 que não contêm informações sobre a origem do problema.</span><span class="sxs-lookup"><span data-stu-id="7da4c-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="7da4c-216">Por exemplo, se o *Views/_ViewImports.cshtml* arquivo contém um namespace que não existe em seu projeto, você obterá um erro HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="7da4c-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="7da4c-217">Por padrão em aplicativos do ASP.NET Core, o `UseDeveloperExceptionPage` extensão é adicionada para o `IApplicationBuilder` e executado quando a configuração é *desenvolvimento*.</span><span class="sxs-lookup"><span data-stu-id="7da4c-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="7da4c-218">Isso é detalhado no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="7da4c-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="7da4c-219">ASP.NET Core converte as exceções sem tratamento em um aplicativo web em respostas de erro HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="7da4c-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="7da4c-220">Normalmente, os detalhes do erro não estão incluídos nessas respostas para evitar a divulgação de informações potencialmente confidenciais sobre o servidor.</span><span class="sxs-lookup"><span data-stu-id="7da4c-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="7da4c-221">Consulte **usando a página de exceção de desenvolvedor** na [tratar erros](../fundamentals/error-handling.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="7da4c-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7da4c-222">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7da4c-222">Additional resources</span></span>

* [<span data-ttu-id="7da4c-223">Desenvolvimento do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="7da4c-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="7da4c-224">Auxiliares de marcação</span><span class="sxs-lookup"><span data-stu-id="7da4c-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
