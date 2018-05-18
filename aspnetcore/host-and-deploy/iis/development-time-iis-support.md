---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: Descobrir o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server.
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
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="7d476-103">Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d476-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="7d476-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7d476-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7d476-105">Este artigo descreve [Visual Studio](https://www.visualstudio.com/vs/) suporte para depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="7d476-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="7d476-106">Este tópico explica como habilitar esse recurso e configuração de um projeto.</span><span class="sxs-lookup"><span data-stu-id="7d476-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d476-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7d476-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="7d476-108">Habilitar o IIS</span><span class="sxs-lookup"><span data-stu-id="7d476-108">Enable IIS</span></span>

1. <span data-ttu-id="7d476-109">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="7d476-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="7d476-110">Selecione o **Internet Information Services** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="7d476-110">Select the **Internet Information Services** check box.</span></span>

![Caixa de seleção do mostrando recursos do Windows Internet Information Services marcada como um quadrado branco (não uma marca de seleção) indicando que alguns dos recursos do IIS estão habilitadas](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="7d476-112">A instalação do IIS pode exigir uma reinicialização do sistema.</span><span class="sxs-lookup"><span data-stu-id="7d476-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="7d476-113">Configurar o IIS</span><span class="sxs-lookup"><span data-stu-id="7d476-113">Configure IIS</span></span>

<span data-ttu-id="7d476-114">O IIS deve ter um site configurado com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7d476-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="7d476-115">Nome do host de um nome de host que corresponda a URL do perfil de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d476-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="7d476-116">Associação para a porta 443, com um certificado atribuídos.</span><span class="sxs-lookup"><span data-stu-id="7d476-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="7d476-117">Por exemplo, o **nome de Host** para um site adicional é definido como "localhost" (o perfil de lançamento também usam "localhost" neste tópico).</span><span class="sxs-lookup"><span data-stu-id="7d476-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="7d476-118">A porta é definida como "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7d476-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="7d476-119">O **certificado do IIS Express desenvolvimento** é atribuído ao site, mas nenhum certificado válido funciona:</span><span class="sxs-lookup"><span data-stu-id="7d476-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Adicione janela de site no IIS, mostrando o conjunto de associação para o localhost na porta 443 com um certificado atribuídos.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="7d476-121">Se a instalação do IIS já tiver um **Default Web Site** com um nome de host que coincide com o nome de host URL do perfil de inicialização do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7d476-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="7d476-122">Adicione uma associação de porta para a porta 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7d476-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="7d476-123">Atribua um certificado válido para o site.</span><span class="sxs-lookup"><span data-stu-id="7d476-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="7d476-124">Habilitar o suporte do IIS de tempo de desenvolvimento no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d476-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="7d476-125">Inicie o instalador do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d476-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="7d476-126">Selecione o **IIS suporte de tempo de desenvolvimento** componente.</span><span class="sxs-lookup"><span data-stu-id="7d476-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="7d476-127">O componente está listado como opcionais no **resumo** painel para o **desenvolvimento ASP.NET e web** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="7d476-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="7d476-128">O componente instala o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos por trás do IIS do ASP.NET Core em uma configuração de proxy reverso.</span><span class="sxs-lookup"><span data-stu-id="7d476-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="7d476-132">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="7d476-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="7d476-133">Redirecionamento de HTTPS</span><span class="sxs-lookup"><span data-stu-id="7d476-133">HTTPS redirection</span></span>

<span data-ttu-id="7d476-134">Para um novo projeto, selecione a caixa de seleção para **configurar para HTTPS** no **novo aplicativo de Web do ASP.NET Core** janela:</span><span class="sxs-lookup"><span data-stu-id="7d476-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nova janela de aplicativo da Web do ASP.NET Core com a configuração para a caixa de seleção HTTPS.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="7d476-136">Em um projeto existente, use HTTPS Middleware de redirecionamento no `Startup.Configure` chamando o [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) método de extensão:</span><span class="sxs-lookup"><span data-stu-id="7d476-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="7d476-137">Inicie o IIS perfil</span><span class="sxs-lookup"><span data-stu-id="7d476-137">IIS launch profile</span></span>

<span data-ttu-id="7d476-138">Crie um novo perfil de lançamento para adicionar suporte ao IIS de tempo de desenvolvimento:</span><span class="sxs-lookup"><span data-stu-id="7d476-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="7d476-139">Para **perfil**, selecione o **novo** botão.</span><span class="sxs-lookup"><span data-stu-id="7d476-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="7d476-140">Nome do perfil "IIS" na janela pop-up.</span><span class="sxs-lookup"><span data-stu-id="7d476-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="7d476-141">Selecione **Okey** para criar o perfil.</span><span class="sxs-lookup"><span data-stu-id="7d476-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="7d476-142">Para o **iniciar** configuração, selecione **IIS** da lista.</span><span class="sxs-lookup"><span data-stu-id="7d476-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="7d476-143">Marque a caixa de seleção **Iniciar navegador** e forneça a URL de ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="7d476-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="7d476-144">Use o protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7d476-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="7d476-145">Este exemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="7d476-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="7d476-146">No **variáveis de ambiente** seção, selecione o **adicionar** botão.</span><span class="sxs-lookup"><span data-stu-id="7d476-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="7d476-147">Fornecer uma variável de ambiente com uma chave de `ASPNETCORE_ENVIRONMENT` e um valor de `Development`.</span><span class="sxs-lookup"><span data-stu-id="7d476-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="7d476-148">No **configurações do servidor Web** área, defina o **URL do aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="7d476-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="7d476-149">Este exemplo usa `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="7d476-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="7d476-150">Salve o perfil.</span><span class="sxs-lookup"><span data-stu-id="7d476-150">Save the profile.</span></span>

![Janela de propriedades de projeto com a guia Depurar selecionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="7d476-155">Como alternativa, um perfil de lançamento para adicionar manualmente o [launchSettings.json](http://json.schemastore.org/launchsettings) arquivo no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7d476-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="7d476-156">Execute o projeto</span><span class="sxs-lookup"><span data-stu-id="7d476-156">Run the project</span></span>

<span data-ttu-id="7d476-157">Na IU do VS, definido no botão executar o **IIS** perfil e selecione o botão para iniciar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7d476-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Botão execute na barra de ferramentas VS definida para o perfil de "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="7d476-159">O Visual Studio pode solicitar uma reinicialização se não executar como administrador.</span><span class="sxs-lookup"><span data-stu-id="7d476-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="7d476-160">Se solicitado, reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d476-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="7d476-161">Se for usado um certificado não confiável de desenvolvimento, o navegador pode exigir a criação de uma exceção para o certificado não confiável.</span><span class="sxs-lookup"><span data-stu-id="7d476-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d476-162">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7d476-162">Additional resources</span></span>

* [<span data-ttu-id="7d476-163">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="7d476-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="7d476-164">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d476-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="7d476-165">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d476-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="7d476-166">Impor o HTTPS</span><span class="sxs-lookup"><span data-stu-id="7d476-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
