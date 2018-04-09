---
title: Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core
author: shirhatti
description: Descobrir o suporte para depuração de aplicativos do ASP.NET Core quando executado por trás do IIS no Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="77d56-103">Suporte ao IIS no tempo de desenvolvimento no Visual Studio para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77d56-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="77d56-104">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="77d56-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="77d56-105">Este artigo descreve [Visual Studio](https://www.visualstudio.com/vs/) suporte para depuração de aplicativos do ASP.NET Core em execução por trás do IIS no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="77d56-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="77d56-106">Este tópico explica como habilitar esse recurso e configuração de um projeto.</span><span class="sxs-lookup"><span data-stu-id="77d56-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77d56-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="77d56-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="77d56-108">Habilitar o IIS</span><span class="sxs-lookup"><span data-stu-id="77d56-108">Enable IIS</span></span>

<span data-ttu-id="77d56-109">Habilite o IIS.</span><span class="sxs-lookup"><span data-stu-id="77d56-109">Enable IIS.</span></span> <span data-ttu-id="77d56-110">Navegue para **Painel de Controle** > **Programas** > **Programas e Recursos** > **Ativar ou desativar recursos do Windows** (lado esquerdo da tela).</span><span class="sxs-lookup"><span data-stu-id="77d56-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="77d56-111">Marque a caixa de seleção **Serviços de Informações da Internet**.</span><span class="sxs-lookup"><span data-stu-id="77d56-111">Select the **Internet Information Services** checkbox.</span></span>

![Recursos do Windows mostrando a caixa de seleção Serviços de Informações da Internet marcada como um quadrado preto (não uma marca de seleção), indicando que alguns dos recursos do IIS estão habilitados](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="77d56-113">Se a instalação do IIS exigir uma reinicialização, reinicie o sistema.</span><span class="sxs-lookup"><span data-stu-id="77d56-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="77d56-114">Habilitar suporte ao IIS no tempo de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="77d56-114">Enable development-time IIS support</span></span>

<span data-ttu-id="77d56-115">Inicie o instalador do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="77d56-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="77d56-116">Selecione o **IIS suporte de tempo de desenvolvimento** componente.</span><span class="sxs-lookup"><span data-stu-id="77d56-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="77d56-117">O componente está listado como opcionais no **resumo** painel para o **desenvolvimento ASP.NET e web** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="77d56-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="77d56-118">Isso instala o [ASP.NET Core módulo](xref:fundamentals/servers/aspnet-core-module), que é um módulo nativo do IIS necessário para executar o ASP.NET Core aplicativos.</span><span class="sxs-lookup"><span data-stu-id="77d56-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modificando os recursos do Visual Studio: a guia Cargas de Trabalho é selecionada.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="77d56-122">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="77d56-122">Configure the project</span></span>

<span data-ttu-id="77d56-123">Crie um novo perfil de inicialização para adicionar suporte ao IIS no tempo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="77d56-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="77d56-124">No **Gerenciador de Soluções** do Visual Studio, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="77d56-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="77d56-125">Selecione a guia **Depurar**. Selecione **IIS** da lista suspensa **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="77d56-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="77d56-126">Confirme se o recurso **Iniciar Navegador** está habilitado com a URL correta.</span><span class="sxs-lookup"><span data-stu-id="77d56-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Janela de propriedades de projeto com a guia Depurar selecionada.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="77d56-131">Como alternativa, um perfil de lançamento para adicionar manualmente o [launchSettings.json](http://json.schemastore.org/launchsettings) arquivo no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="77d56-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="77d56-132">O Visual Studio pode solicitar uma reinicialização se não executar como administrador.</span><span class="sxs-lookup"><span data-stu-id="77d56-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="77d56-133">Se solicitado, reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="77d56-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77d56-134">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="77d56-134">Additional resources</span></span>

* [<span data-ttu-id="77d56-135">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="77d56-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="77d56-136">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77d56-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="77d56-137">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77d56-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
