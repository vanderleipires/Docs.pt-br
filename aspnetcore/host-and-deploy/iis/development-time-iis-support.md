---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="4119e-103">Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4119e-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="4119e-104">Por: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="4119e-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="4119e-105">Este artigo descreve o suporte do [Visual Studio](https://www.visualstudio.com/vs/) a depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="4119e-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="4119e-106">Este tópico explica como habilitar esse recurso e configuração de um projeto.</span><span class="sxs-lookup"><span data-stu-id="4119e-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4119e-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="4119e-107">Prerequisites</span></span>

* <span data-ttu-id="4119e-108">Visual Studio (2017/versão 15.3 ou posterior)</span><span class="sxs-lookup"><span data-stu-id="4119e-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="4119e-109">Carga de trabalho de desenvolvimento Web e ASP.NET *OU* a carga de trabalho de desenvolvimento multiplataforma do .NET Core</span><span class="sxs-lookup"><span data-stu-id="4119e-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="4119e-110">Habilitar o IIS</span><span class="sxs-lookup"><span data-stu-id="4119e-110">Enable IIS</span></span>

<span data-ttu-id="4119e-111">Habilite o IIS.</span><span class="sxs-lookup"><span data-stu-id="4119e-111">Enable IIS.</span></span> <span data-ttu-id="4119e-112">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="4119e-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="4119e-113">Marque a caixa de seleção **Serviços de Informações da Internet**.</span><span class="sxs-lookup"><span data-stu-id="4119e-113">Select the **Internet Information Services** checkbox.</span></span>

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="4119e-115">Se a instalação do IIS requer uma reinicialização, reinicie o sistema.</span><span class="sxs-lookup"><span data-stu-id="4119e-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="4119e-116">Habilitar suporte ao IIS no tempo de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="4119e-116">Enable development-time IIS support</span></span>

<span data-ttu-id="4119e-117">Depois que o IIS estiver instalado, inicie o instalador do Visual Studio para modificar a instalação existente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4119e-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="4119e-118">No instalador, selecione o componente **Suporte ao IIS no tempo de desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="4119e-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="4119e-119">O componente está listado como um componente opcional do painel **Resumo** para a carga de trabalho **Desenvolvimento Web e ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="4119e-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="4119e-120">Isso instala o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4119e-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="4119e-124">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="4119e-124">Configure the project</span></span>

<span data-ttu-id="4119e-125">Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="4119e-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="4119e-126">No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="4119e-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="4119e-127">Selecione a guia **Depurar**. Selecione **IIS** da lista suspensa **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="4119e-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="4119e-128">Confirme se o recurso **Iniciar Navegador** está habilitado com a URL correta.</span><span class="sxs-lookup"><span data-stu-id="4119e-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Janela de propriedades de projeto com a guia Depurar selecionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="4119e-133">Como alternativa, um perfil de lançamento para adicionar manualmente o [launchSettings.json](http://json.schemastore.org/launchsettings) arquivo no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4119e-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="4119e-134">O Visual Studio pode solicitar uma reinicialização se não executar como administrador.</span><span class="sxs-lookup"><span data-stu-id="4119e-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="4119e-135">Se solicitado, reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4119e-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="4119e-136">Parabéns!</span><span class="sxs-lookup"><span data-stu-id="4119e-136">Congratulations!</span></span> <span data-ttu-id="4119e-137">Neste ponto, o projeto está configurado para suporte de tempo de desenvolvimento do IIS.</span><span class="sxs-lookup"><span data-stu-id="4119e-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4119e-138">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4119e-138">Additional resources</span></span>

* [<span data-ttu-id="4119e-139">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="4119e-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="4119e-140">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4119e-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="4119e-141">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4119e-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
