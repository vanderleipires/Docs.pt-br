---
title: Implantar aplicativos ASP.NET Core no Serviço de Aplicativo do Azure
author: guardrex
description: Este artigo contém links para o host do Azure e para implantar recursos.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: f2de81af4bd2992aec76a287484d0057021231d8
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860960"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="aa905-103">Implantar aplicativos ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="aa905-104">[Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) é um [serviço de plataforma de computação em nuvem da Microsoft](https://azure.microsoft.com/) para hospedar aplicativos Web, incluindo o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa905-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="aa905-105">Recursos úteis</span><span class="sxs-lookup"><span data-stu-id="aa905-105">Useful resources</span></span>

<span data-ttu-id="aa905-106">A [Documentação de Aplicativos Web](/azure/app-service/) do Azure é a página inicial para documentação de Azure Apps, tutoriais, exemplos, guias de instruções e outros recursos.</span><span class="sxs-lookup"><span data-stu-id="aa905-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="aa905-107">Dois tutoriais importantes que pertencem à hospedagem de aplicativos ASP.NET Core são:</span><span class="sxs-lookup"><span data-stu-id="aa905-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="aa905-108">Início rápido: Criar um aplicativo Web ASP.NET Core no Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="aa905-109">Use o Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Windows.</span><span class="sxs-lookup"><span data-stu-id="aa905-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="aa905-110">Início rápido: Criar um aplicativo Web .NET Core no Serviço de Aplicativo no Linux</span><span class="sxs-lookup"><span data-stu-id="aa905-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="aa905-111">Use a linha de comando do Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Linux.</span><span class="sxs-lookup"><span data-stu-id="aa905-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="aa905-112">Os artigos a seguir estão disponíveis na documentação do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="aa905-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="aa905-113">Publicar no Azure com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa905-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="aa905-114">Aprenda como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa905-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="aa905-115">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="aa905-115">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="aa905-116">Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua.</span><span class="sxs-lookup"><span data-stu-id="aa905-116">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="aa905-117">Criar seu primeiro pipeline com o Azure Pipelines</span><span class="sxs-lookup"><span data-stu-id="aa905-117">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="aa905-118">Configurar um build de CI para um aplicativo ASP.NET Core e, em seguida, criar uma versão de implantação contínua para o Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa905-118">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="aa905-119">Área restrita de aplicativo Web do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-119">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="aa905-120">Descubra as limitações de tempo de execução do Serviço de Aplicativo do Azure impostas pela plataforma de Aplicativos do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa905-120">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="aa905-121">Configuração do aplicativo</span><span class="sxs-lookup"><span data-stu-id="aa905-121">Application configuration</span></span>

<span data-ttu-id="aa905-122">No ASP.NET Core 2.0 ou posterior, os seguintes pacotes NuGet fornecem recursos de registro em log automático para aplicativos implantados para o Serviço de Aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="aa905-122">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="aa905-123">O [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) para fornecer integração leve do ASP.NET Core com o Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa905-123">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="aa905-124">Os recursos de registro em log adicionais são fornecidos pelo pacote `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="aa905-124">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="aa905-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executa [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para adicionar provedores de log de diagnósticos do Serviço de Aplicativo do Azure no pacote `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="aa905-125">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="aa905-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fornece implementações de agente para dar suporte a recursos de streaming de log e logs de diagnóstico do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa905-126">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="aa905-127">Se você estiver direcionando para o .NET Core e estiver referenciando o [metapacote Microsoft.AspNetCore.All](xref:fundamentals/metapackage), os pacotes já estarão incluídos.</span><span class="sxs-lookup"><span data-stu-id="aa905-127">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="aa905-128">Ambos os pacotes estão ausentes no [metapacote Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) mais novo.</span><span class="sxs-lookup"><span data-stu-id="aa905-128">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="aa905-129">Se você estiver direcionando ao .NET Framework ou referenciando ao metapacote `Microsoft.AspNetCore.App`, referencie os pacotes individuais de registro em log.</span><span class="sxs-lookup"><span data-stu-id="aa905-129">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="aa905-130">Substituir a configuração do aplicativo no Portal do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-130">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="aa905-131">A área **Configurações de aplicativo** da folha **Configurações de aplicativo** permite definir variáveis de ambiente para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa905-131">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="aa905-132">As variáveis de ambiente podem ser consumidas pelo [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="aa905-132">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="aa905-133">Quando o aplicativo usa o [host da Web](xref:fundamentals/host/web-host) e cria o host usando [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), as variáveis de ambiente que configuram o host usam o prefixo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="aa905-133">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="aa905-134">Para saber mais, confira <xref:fundamentals/host/web-host> e o [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="aa905-134">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="aa905-135">Quando o aplicativo usa o [host genérico](xref:fundamentals/host/generic-host), as variáveis de ambiente não são carregadas na configuração do aplicativo por padrão, e o provedor de configuração deve ser adicionado pelo desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="aa905-135">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="aa905-136">O desenvolvedor determina o prefixo da variável de ambiente quando o provedor de configuração é adicionado.</span><span class="sxs-lookup"><span data-stu-id="aa905-136">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="aa905-137">Para saber mais, confira <xref:fundamentals/host/generic-host> e o [Provedor de configuração de variáveis de ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="aa905-137">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="aa905-138">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="aa905-138">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="aa905-139">O Middleware de integração do IIS, que configura Middleware de cabeçalhos encaminhados, e o módulo do ASP.NET Core são configurados para encaminhar o esquema (HTTP/HTTPS) e o endereço IP remoto de onde a solicitação foi originada.</span><span class="sxs-lookup"><span data-stu-id="aa905-139">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="aa905-140">Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy adicionais e balanceadores de carga.</span><span class="sxs-lookup"><span data-stu-id="aa905-140">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="aa905-141">Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="aa905-141">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="aa905-142">Monitoramento e registro em log</span><span class="sxs-lookup"><span data-stu-id="aa905-142">Monitoring and logging</span></span>

<span data-ttu-id="aa905-143">Para monitoramento, registro em log e informações de solução de problemas, veja os seguintes artigos:</span><span class="sxs-lookup"><span data-stu-id="aa905-143">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="aa905-144">Como: monitorar aplicativos no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-144">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="aa905-145">Saiba como examinar as cotas e métricas para aplicativos e planos do Serviço de Aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa905-145">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="aa905-146">Habilitar log de diagnósticos para aplicativos Web no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-146">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="aa905-147">Descubra como habilitar e acessar o log de diagnósticos para os códigos de status HTTP, solicitações com falha e atividade do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="aa905-147">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="aa905-148">Introdução ao tratamento de erro no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa905-148">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="aa905-149">Entenda as abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa905-149">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="aa905-150">Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-150">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="aa905-151">Saiba como diagnosticar problemas com implantações do Serviço de Aplicativo do Azure com aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa905-151">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="aa905-152">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa905-152">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="aa905-153">Consulte os erros comuns de configuração de implantação para aplicativos hospedados pelo Serviço de Aplicativo do Azure/IIS com orientação para solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="aa905-153">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="aa905-154">Anel de chave de proteção de dados e slots de implantação</span><span class="sxs-lookup"><span data-stu-id="aa905-154">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="aa905-155">[Chaves de proteção de dados](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) são mantidas na pasta *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="aa905-155">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="aa905-156">Essa pasta é apoiada pelo repositório de rede e é sincronizada em todos os computadores que hospedam o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa905-156">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="aa905-157">As chaves não são protegidas em repouso.</span><span class="sxs-lookup"><span data-stu-id="aa905-157">Keys aren't protected at rest.</span></span> <span data-ttu-id="aa905-158">Essa pasta fornece o anel de chave para todas as instâncias de um aplicativo em um único slot de implantação.</span><span class="sxs-lookup"><span data-stu-id="aa905-158">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="aa905-159">Slots de implantação separados, como de preparo e produção, não compartilham um anel de chave.</span><span class="sxs-lookup"><span data-stu-id="aa905-159">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="aa905-160">Quando ocorre a troca entre os slots de implantação, nenhum sistema que usa a proteção de dados consegue descriptografar dados armazenados usando o anel de chave dentro do slot anterior.</span><span class="sxs-lookup"><span data-stu-id="aa905-160">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="aa905-161">O middleware de cookie do ASP.NET usa a proteção de dados para proteger seus cookies.</span><span class="sxs-lookup"><span data-stu-id="aa905-161">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="aa905-162">Com isso, os usuários são desconectados de um aplicativo que usa o Middleware de Cookie padrão do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aa905-162">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="aa905-163">Para uma solução de anel de chave independente de slot, use um provedor de anel de chave externo, como:</span><span class="sxs-lookup"><span data-stu-id="aa905-163">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="aa905-164">Armazenamento do Blobs do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-164">Azure Blob Storage</span></span>
* <span data-ttu-id="aa905-165">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="aa905-165">Azure Key Vault</span></span>
* <span data-ttu-id="aa905-166">Repositório SQL</span><span class="sxs-lookup"><span data-stu-id="aa905-166">SQL store</span></span>
* <span data-ttu-id="aa905-167">Cache redis</span><span class="sxs-lookup"><span data-stu-id="aa905-167">Redis cache</span></span>

<span data-ttu-id="aa905-168">Para obter mais informações, veja [Principais provedores de armazenamento](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="aa905-168">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="aa905-169">Implantar a versão de visualização do ASP.NET Core para o Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-169">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="aa905-170">Aplicativos de visualização do ASP.NET Core podem ser implantados para o Serviço de Aplicativo do Azure com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="aa905-170">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="aa905-171">[Instalar a extensão de site da versão prévia](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="aa905-171">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="aa905-172">Usar o Docker com aplicativos Web para contêineres</span><span class="sxs-lookup"><span data-stu-id="aa905-172">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="aa905-173">Instalar a extensão de site de visualização</span><span class="sxs-lookup"><span data-stu-id="aa905-173">Install the preview site extension</span></span>

<span data-ttu-id="aa905-174">Se houver problemas ao usar a extensão de site de visualização, abra um problema no [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="aa905-174">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="aa905-175">No portal do Azure, navegue até a folha Serviço de Aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa905-175">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="aa905-176">Selecione o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="aa905-176">Select the web app.</span></span>
1. <span data-ttu-id="aa905-177">Insira "ex" na caixa de pesquisa ou role para baixo na lista de seções de gerenciamento para **FERRAMENTAS DE DESENVOLVIMENTO**.</span><span class="sxs-lookup"><span data-stu-id="aa905-177">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="aa905-178">Selecione **FERRAMENTAS DE DESENVOLVIMENTO** > **Extensões**.</span><span class="sxs-lookup"><span data-stu-id="aa905-178">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="aa905-179">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="aa905-179">Select **Add**.</span></span>
1. <span data-ttu-id="aa905-180">Selecione a extensão de **Tempo de execução do ASP.NET Core &lt;x.y&gt; (x86)** na lista, em que `<x.y>` é a versão prévia do ASP.NET Core (por exemplo, **Tempo de execução do ASP.NET Core 2.2 (x86)**).</span><span class="sxs-lookup"><span data-stu-id="aa905-180">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="aa905-181">O tempo de execução x86 é apropriado para [implantações dependentes de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) que dependem de hospedagem de fora do processo pelo Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa905-181">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="aa905-182">Selecione **OK** para aceitar os termos legais.</span><span class="sxs-lookup"><span data-stu-id="aa905-182">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="aa905-183">Selecione **OK** para instalar a extensão.</span><span class="sxs-lookup"><span data-stu-id="aa905-183">Select **OK** to install the extension.</span></span>

<span data-ttu-id="aa905-184">Quando a operação for concluída, a versão prévia mais recente do .NET Core será instalada.</span><span class="sxs-lookup"><span data-stu-id="aa905-184">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="aa905-185">Verifique a instalação:</span><span class="sxs-lookup"><span data-stu-id="aa905-185">Verify the installation:</span></span>

1. <span data-ttu-id="aa905-186">Selecione **Ferramentas Avançadas** sob **FERRAMENTAS DE DESENVOLVIMENTO**.</span><span class="sxs-lookup"><span data-stu-id="aa905-186">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="aa905-187">Selecione **Ir** na folha **Ferramentas Avançadas**.</span><span class="sxs-lookup"><span data-stu-id="aa905-187">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="aa905-188">Selecione o item de menu **Console de depuração** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="aa905-188">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="aa905-189">No prompt do PowerShell, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="aa905-189">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="aa905-190">Substitua a versão de tempo de execução do ASP.NET Core por `<x.y>` no comando:</span><span class="sxs-lookup"><span data-stu-id="aa905-190">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="aa905-191">Se o tempo de execução da versão prévia instalada for ASP.NET Core 2.2, o comando será:</span><span class="sxs-lookup"><span data-stu-id="aa905-191">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="aa905-192">O comando retornará `True` quando o tempo de execução da versão prévia x64 estiver instalado.</span><span class="sxs-lookup"><span data-stu-id="aa905-192">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="aa905-193">A arquitetura da plataforma (x86/x64) de um aplicativo de Serviços de Aplicativos é definida na folha **Configurações do Aplicativo** em **Configurações Gerais** para aplicativos que são hospedados em uma computação de série A ou em uma melhor camada de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="aa905-193">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="aa905-194">Se o aplicativo for executado no modo em processo e a arquitetura da plataforma estiver configurada para 64 bits (x64), o Módulo do ASP.NET Core usará o tempo de execução da versão prévia de 64 bits, se estiver presente.</span><span class="sxs-lookup"><span data-stu-id="aa905-194">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="aa905-195">Instalar a extensão **Tempo de execução do ASP.NET Core &lt;x.y&gt; (x64)** (por exemplo, **Tempo de execução do ASP.NET Core 2.2 (x64)**).</span><span class="sxs-lookup"><span data-stu-id="aa905-195">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="aa905-196">Depois de instalar o tempo de execução da versão prévia x64, execute o seguinte comando na janela de comando do Kudu PowerShell para verificar a instalação.</span><span class="sxs-lookup"><span data-stu-id="aa905-196">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="aa905-197">Substitua a versão de tempo de execução do ASP.NET Core por `<x.y>` no comando:</span><span class="sxs-lookup"><span data-stu-id="aa905-197">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="aa905-198">Se o tempo de execução da versão prévia instalada for ASP.NET Core 2.2, o comando será:</span><span class="sxs-lookup"><span data-stu-id="aa905-198">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="aa905-199">O comando retornará `True` quando o tempo de execução da versão prévia x64 estiver instalado.</span><span class="sxs-lookup"><span data-stu-id="aa905-199">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="aa905-200">As **Extensões do ASP.NET Core** habilitam uma funcionalidade adicional para o ASP.NET Core nos Serviços de Aplicativo do Azure, como a habilitação do registro em log do Azure.</span><span class="sxs-lookup"><span data-stu-id="aa905-200">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="aa905-201">A extensão é instalada automaticamente durante a implantação do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa905-201">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="aa905-202">Se a extensão não estiver instalada, instale-a para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="aa905-202">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="aa905-203">**Usar a extensão de site de visualização com um modelo do ARM**</span><span class="sxs-lookup"><span data-stu-id="aa905-203">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="aa905-204">Se um modelo do ARM for usado para criar e implantar aplicativos, o tipo de recurso `siteextensions` poderá ser usado para adicionar a extensão de site a um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="aa905-204">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="aa905-205">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="aa905-205">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="aa905-206">Usar o Docker com aplicativos Web para contêineres</span><span class="sxs-lookup"><span data-stu-id="aa905-206">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="aa905-207">O [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contém as imagens de versão prévia do Docker mais recentes.</span><span class="sxs-lookup"><span data-stu-id="aa905-207">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="aa905-208">As imagens podem ser usadas como uma imagem de base.</span><span class="sxs-lookup"><span data-stu-id="aa905-208">The images can be used as a base image.</span></span> <span data-ttu-id="aa905-209">Use a imagem e implante aplicativos Web para contêineres normalmente.</span><span class="sxs-lookup"><span data-stu-id="aa905-209">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa905-210">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aa905-210">Additional resources</span></span>

* [<span data-ttu-id="aa905-211">Visão geral de aplicativos Web (vídeo de visão geral com 5 minutos)</span><span class="sxs-lookup"><span data-stu-id="aa905-211">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="aa905-212">Serviço de Aplicativo do Azure: o melhor lugar para hospedar seus aplicativos .NET (vídeo de visão geral com 55 minutos)</span><span class="sxs-lookup"><span data-stu-id="aa905-212">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="aa905-213">Azure Friday: experiência de diagnóstico e solução de problemas do Serviço de Aplicativo do Azure (vídeo com 12 minutos)</span><span class="sxs-lookup"><span data-stu-id="aa905-213">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="aa905-214">Visão geral de diagnóstico do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="aa905-214">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="aa905-215">O Serviço de Aplicativo do Azure no Windows Server usa o [IIS (Serviços de Informações da Internet)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="aa905-215">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="aa905-216">Os tópicos a seguir estão relacionados com a tecnologia subjacente do IIS:</span><span class="sxs-lookup"><span data-stu-id="aa905-216">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="aa905-217">Biblioteca Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="aa905-217">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
