---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: Descubra o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233073"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="49c45-103">Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49c45-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="49c45-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="49c45-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="49c45-105">Este artigo descreve o suporte do [Visual Studio](https://www.visualstudio.com/vs/) a depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="49c45-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="49c45-106">Este tópico orienta você sobre como habilitar esse recurso e configurar um projeto.</span><span class="sxs-lookup"><span data-stu-id="49c45-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49c45-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="49c45-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="49c45-108">Habilitar o IIS</span><span class="sxs-lookup"><span data-stu-id="49c45-108">Enable IIS</span></span>

1. <span data-ttu-id="49c45-109">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="49c45-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="49c45-110">Selecione a caixa de seleção **Serviços de Informações da Internet**.</span><span class="sxs-lookup"><span data-stu-id="49c45-110">Select the **Internet Information Services** check box.</span></span>

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="49c45-112">A instalação do IIS pode exigir uma reinicialização do sistema.</span><span class="sxs-lookup"><span data-stu-id="49c45-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="49c45-113">Configurar o IIS</span><span class="sxs-lookup"><span data-stu-id="49c45-113">Configure IIS</span></span>

<span data-ttu-id="49c45-114">O IIS deve ter um site configurado com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="49c45-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="49c45-115">Um nome do host que corresponda ao nome do host da URL do perfil de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="49c45-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="49c45-116">Associação para a porta 443, com um certificado atribuído.</span><span class="sxs-lookup"><span data-stu-id="49c45-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="49c45-117">Por exemplo, o **Nome do host** para um site adicionado é definido como "localhost" (o perfil de inicialização também usará "localhost" posteriormente neste tópico).</span><span class="sxs-lookup"><span data-stu-id="49c45-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="49c45-118">A porta é definida para "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="49c45-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="49c45-119">O **Certificado de Desenvolvimento do IIS Express** é atribuído ao site, mas nenhum certificado válido funciona:</span><span class="sxs-lookup"><span data-stu-id="49c45-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Adicione a janela Site no IIS, mostrando o conjunto de associação para o localhost na porta 443 com um certificado atribuído.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="49c45-121">Se a instalação do IIS já tiver um **site da Web padrão** com um nome do host que corresponde ao nome do host da URL do perfil de inicialização do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="49c45-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="49c45-122">Adicione uma associação de porta para a porta 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="49c45-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="49c45-123">Atribua um certificado válido para o site.</span><span class="sxs-lookup"><span data-stu-id="49c45-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="49c45-124">Habilitar o suporte ao IIS no tempo de desenvolvimento no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49c45-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="49c45-125">Inicie o Instalador do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49c45-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="49c45-126">Selecione o componente **Suporte ao IIS no tempo de desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="49c45-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="49c45-127">O componente está listado como opcional no painel **Resumo** para a carga de trabalho **Desenvolvimento Web e ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="49c45-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="49c45-128">O componente instala o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos do ASP.NET Core por trás de IIS em uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="49c45-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="49c45-132">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="49c45-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="49c45-133">Redirecionamento para HTTPS</span><span class="sxs-lookup"><span data-stu-id="49c45-133">HTTPS redirection</span></span>

<span data-ttu-id="49c45-134">Para um novo projeto, selecione a caixa de seleção para **Configurar para HTTPS** na janela **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="49c45-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Janela Novo Aplicativo Web ASP.NET Core com a caixa de seleção Configurar para HTTPS selecionada.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="49c45-136">Em um projeto existente, use o middleware de redirecionamento para HTTPS no `Startup.Configure` chamando o método de extensão [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="49c45-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="49c45-137">Perfil de inicialização do IIS</span><span class="sxs-lookup"><span data-stu-id="49c45-137">IIS launch profile</span></span>

<span data-ttu-id="49c45-138">Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento:</span><span class="sxs-lookup"><span data-stu-id="49c45-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="49c45-139">Para **Perfil**, selecione o botão **Novo**.</span><span class="sxs-lookup"><span data-stu-id="49c45-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="49c45-140">Nomeie o perfil "IIS" na janela pop-up.</span><span class="sxs-lookup"><span data-stu-id="49c45-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="49c45-141">Selecione **OK** para criar o perfil.</span><span class="sxs-lookup"><span data-stu-id="49c45-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="49c45-142">Para a configuração **Iniciar**, selecione **IIS** da lista.</span><span class="sxs-lookup"><span data-stu-id="49c45-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="49c45-143">Selecione a caixa de seleção **Iniciar navegador** e forneça a URL de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="49c45-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="49c45-144">Use o protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="49c45-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="49c45-145">Este exemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="49c45-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="49c45-146">Na seção **Variáveis de ambiente**, selecione o botão **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="49c45-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="49c45-147">Fornecer uma variável de ambiente com uma chave `ASPNETCORE_ENVIRONMENT` e um valor `Development`.</span><span class="sxs-lookup"><span data-stu-id="49c45-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="49c45-148">Na área **Configurações do Servidor Web**, defina a **URL do Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="49c45-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="49c45-149">Este exemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="49c45-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="49c45-150">Salve o perfil.</span><span class="sxs-lookup"><span data-stu-id="49c45-150">Save the profile.</span></span>

![Janela de propriedades de projeto com a guia Depurar selecionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="49c45-155">Como alternativa, adicione manualmente um perfil de inicialização para o arquivo [launchSettings.json](http://json.schemastore.org/launchsettings) no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="49c45-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a><span data-ttu-id="49c45-156">Executar o projeto</span><span class="sxs-lookup"><span data-stu-id="49c45-156">Run the project</span></span>

<span data-ttu-id="49c45-157">Na interface do usuário do VS, definido no botão executar o **IIS** perfil e selecione o botão para iniciar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="49c45-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Botão de execução na barra de ferramentas do VS definido para o perfil "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="49c45-159">O Visual Studio poderá solicitar uma reinicialização se não estiver executando como administrador.</span><span class="sxs-lookup"><span data-stu-id="49c45-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="49c45-160">Se solicitado, reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49c45-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="49c45-161">Se for usado um certificado de desenvolvimento não confiável, o navegador poderá exigir a criação de uma exceção para o certificado não confiável.</span><span class="sxs-lookup"><span data-stu-id="49c45-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49c45-162">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="49c45-162">Additional resources</span></span>

* [<span data-ttu-id="49c45-163">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="49c45-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="49c45-164">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49c45-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="49c45-165">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49c45-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="49c45-166">Impor o HTTPS</span><span class="sxs-lookup"><span data-stu-id="49c45-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
