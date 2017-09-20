---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server."
keywords: "ASP.NET Core, Serviços de Informações da Internet, IIS, Windows Server, módulo do ASP.NET Core, depuração"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: f531d90646b9d261c5fbbffcecd6ded9185ae292
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="aab36-104">Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aab36-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="aab36-105">Por: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="aab36-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="aab36-106">Este artigo descreve o suporte do [Visual Studio](https://www.visualstudio.com/vs/) a depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="aab36-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="aab36-107">Este tópico orienta você sobre como habilitar esse recurso e configurar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="aab36-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aab36-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="aab36-108">Prerequisites</span></span>

* <span data-ttu-id="aab36-109">Visual Studio (2017/versão 15.3 ou posterior)</span><span class="sxs-lookup"><span data-stu-id="aab36-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="aab36-110">Carga de trabalho de desenvolvimento Web e ASP.NET *OU* a carga de trabalho de desenvolvimento multiplataforma do .NET Core</span><span class="sxs-lookup"><span data-stu-id="aab36-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="aab36-111">Habilitar o IIS</span><span class="sxs-lookup"><span data-stu-id="aab36-111">Enable IIS</span></span>

<span data-ttu-id="aab36-112">Habilite o IIS no seu sistema.</span><span class="sxs-lookup"><span data-stu-id="aab36-112">Enable IIS on your system.</span></span> <span data-ttu-id="aab36-113">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="aab36-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="aab36-114">Marque a caixa de seleção **Serviços de Informações da Internet**.</span><span class="sxs-lookup"><span data-stu-id="aab36-114">Select the **Internet Information Services** checkbox.</span></span>

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="aab36-116">Se a instalação do IIS requer uma reinicialização, reinicie o sistema.</span><span class="sxs-lookup"><span data-stu-id="aab36-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="aab36-117">Habilitar suporte ao IIS no tempo de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="aab36-117">Enable development-time IIS support</span></span>

<span data-ttu-id="aab36-118">Depois de instalar o IIS, inicie o instalador do Visual Studio para modificar sua instalação existente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aab36-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="aab36-119">No instalador, selecione o componente **Suporte ao IIS no tempo de desenvolvimento**.</span><span class="sxs-lookup"><span data-stu-id="aab36-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="aab36-120">O componente está listado como um componente opcional do painel **Resumo** para a carga de trabalho **Desenvolvimento Web e ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="aab36-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="aab36-121">Isso instala o [Módulo do ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar aplicativos do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aab36-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="aab36-125">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="aab36-125">Configure the project</span></span>

<span data-ttu-id="aab36-126">Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="aab36-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="aab36-127">No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="aab36-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="aab36-128">Selecione a guia **Depurar**. Selecione **IIS** da lista suspensa **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="aab36-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="aab36-129">Confirme se o recurso **Iniciar Navegador** está habilitado com a URL correta.</span><span class="sxs-lookup"><span data-stu-id="aab36-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Janela de propriedades de projeto com a guia Depurar selecionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="aab36-134">Como alternativa, você pode adicionar manualmente um perfil de inicialização para o arquivo [launchSettings.json](http://json.schemastore.org/launchsettings) no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="aab36-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="aab36-135">Pode ser solicitado que você reinicie o Visual Studio se você não estava em executando como administrador.</span><span class="sxs-lookup"><span data-stu-id="aab36-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="aab36-136">Se solicitado, reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aab36-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="aab36-137">Parabéns!</span><span class="sxs-lookup"><span data-stu-id="aab36-137">Congratulations!</span></span> <span data-ttu-id="aab36-138">Neste ponto, seu projeto está configurado para suporte ao IIS no tempo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="aab36-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="aab36-139">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aab36-139">Additional resources</span></span>

* [<span data-ttu-id="aab36-140">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="aab36-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="aab36-141">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aab36-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="aab36-142">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aab36-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
