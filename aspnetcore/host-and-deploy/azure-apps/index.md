---
title: Hospedar o ASP.NET Core no Serviço de Aplicativo do Azure
author: guardrex
description: Descubra como hospedar aplicativos ASP.NET Core no Serviço de Aplicativo do Azure com links para recursos úteis.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="7fdaa-103">Hospedar o ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="7fdaa-104">[Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) é um [serviço de plataforma de computação em nuvem da Microsoft](https://azure.microsoft.com/) para hospedar aplicativos Web, incluindo o ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="7fdaa-105">Recursos úteis</span><span class="sxs-lookup"><span data-stu-id="7fdaa-105">Useful resources</span></span>

<span data-ttu-id="7fdaa-106">A [Documentação de Aplicativos Web](/azure/app-service/) do Azure é a página inicial para documentação de Azure Apps, tutoriais, exemplos, guias de instruções e outros recursos.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="7fdaa-107">Dois tutoriais importantes que pertencem à hospedagem de aplicativos ASP.NET Core são:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="7fdaa-108">Início rápido: Criar um aplicativo Web ASP.NET Core no Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="7fdaa-109">Use o Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Windows.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="7fdaa-110">Início rápido: Criar um aplicativo Web .NET Core no Serviço de Aplicativo no Linux</span><span class="sxs-lookup"><span data-stu-id="7fdaa-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="7fdaa-111">Use a linha de comando do Visual Studio para criar e implantar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure no Linux.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="7fdaa-112">Os artigos a seguir estão disponíveis na documentação do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="7fdaa-113">Publicar no Azure com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fdaa-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="7fdaa-114">Aprenda como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="7fdaa-115">Publicar no Azure com as ferramentas CLI</span><span class="sxs-lookup"><span data-stu-id="7fdaa-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="7fdaa-116">Saiba como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o cliente de linha de comando do Git.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="7fdaa-117">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="7fdaa-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="7fdaa-118">Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="7fdaa-119">Implantação contínua no Azure com o VSTS</span><span class="sxs-lookup"><span data-stu-id="7fdaa-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="7fdaa-120">Configurar um build de CI para um aplicativo ASP.NET Core e, em seguida, criar uma versão de implantação contínua para o Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="7fdaa-121">Área restrita de aplicativo Web do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="7fdaa-122">Descubra as limitações de tempo de execução do Serviço de Aplicativo do Azure impostas pela plataforma de Aplicativos do Azure.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="7fdaa-123">Configuração do aplicativo</span><span class="sxs-lookup"><span data-stu-id="7fdaa-123">Application configuration</span></span>

<span data-ttu-id="7fdaa-124">Com o ASP.NET Core 2.0 e posterior, três pacotes no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) fornecem recursos de registro em log automático para aplicativos implantados no Serviço de Aplicativo do Azure:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="7fdaa-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) usa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) para fornecer integração leve do ASP.NET Core com o Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="7fdaa-126">Os recursos de registro em log adicionais são fornecidos pelo pacote `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="7fdaa-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executa [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) para adicionar provedores de log de diagnósticos do Serviço de Aplicativo do Azure no pacote `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="7fdaa-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fornece implementações de agente para dar suporte a recursos de streaming de log e logs de diagnóstico do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="7fdaa-129">Servidor proxy e cenários de balanceador de carga</span><span class="sxs-lookup"><span data-stu-id="7fdaa-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="7fdaa-130">O Middleware de integração do IIS, que configura Middleware de cabeçalhos encaminhados, e o módulo do ASP.NET Core são configurados para encaminhar o esquema (HTTP/HTTPS) e o endereço IP remoto de onde a solicitação foi originada.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="7fdaa-131">Configuração adicional pode ser necessária para aplicativos hospedados atrás de servidores proxy adicionais e balanceadores de carga.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="7fdaa-132">Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="7fdaa-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="7fdaa-133">Monitoramento e registro em log</span><span class="sxs-lookup"><span data-stu-id="7fdaa-133">Monitoring and logging</span></span>

<span data-ttu-id="7fdaa-134">Para monitoramento, registro em log e informações de solução de problemas, veja os seguintes artigos:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="7fdaa-135">Como: monitorar aplicativos no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="7fdaa-136">Saiba como examinar as cotas e métricas para aplicativos e planos do Serviço de Aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="7fdaa-137">Habilitar log de diagnósticos para aplicativos Web no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="7fdaa-138">Descubra como habilitar e acessar o log de diagnósticos para os códigos de status HTTP, solicitações com falha e atividade do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="7fdaa-139">Introdução ao tratamento de erro no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdaa-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="7fdaa-140">Entenda abordagens comuns para o tratamento de erros em aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="7fdaa-141">Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="7fdaa-142">Saiba como diagnosticar problemas com implantações do Serviço de Aplicativo do Azure com aplicativos ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="7fdaa-143">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdaa-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="7fdaa-144">Consulte os erros comuns de configuração de implantação para aplicativos hospedados pelo Serviço de Aplicativo do Azure/IIS com orientação para solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="7fdaa-145">Anel de chave de proteção de dados e slots de implantação</span><span class="sxs-lookup"><span data-stu-id="7fdaa-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="7fdaa-146">[Chaves de proteção de dados](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) são mantidas na pasta *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="7fdaa-147">Essa pasta é apoiada pelo repositório de rede e é sincronizada em todos os computadores que hospedam o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="7fdaa-148">As chaves não são protegidas em repouso.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="7fdaa-149">Essa pasta fornece o anel de chave para todas as instâncias de um aplicativo em um único slot de implantação.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="7fdaa-150">Slots de implantação separados, como de preparo e produção, não compartilham um anel de chave.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="7fdaa-151">Quando ocorre a troca entre os slots de implantação, nenhum sistema que usa a proteção de dados consegue descriptografar dados armazenados usando o anel de chave dentro do slot anterior.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="7fdaa-152">O middleware de cookie do ASP.NET usa a proteção de dados para proteger seus cookies.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="7fdaa-153">Com isso, os usuários são desconectados de um aplicativo que usa o Middleware de Cookie padrão do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="7fdaa-154">Para uma solução de anel de chave independente de slot, use um provedor de anel de chave externo, como:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="7fdaa-155">Armazenamento do Blobs do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-155">Azure Blob Storage</span></span>
* <span data-ttu-id="7fdaa-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7fdaa-156">Azure Key Vault</span></span>
* <span data-ttu-id="7fdaa-157">Repositório SQL</span><span class="sxs-lookup"><span data-stu-id="7fdaa-157">SQL store</span></span>
* <span data-ttu-id="7fdaa-158">Cache redis</span><span class="sxs-lookup"><span data-stu-id="7fdaa-158">Redis cache</span></span>

<span data-ttu-id="7fdaa-159">Para obter mais informações, veja [Principais provedores de armazenamento](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="7fdaa-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="7fdaa-160">Implantar a versão de visualização do ASP.NET Core para o Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="7fdaa-161">Aplicativos de visualização do ASP.NET Core podem ser implantados para o Serviço de Aplicativo do Azure com as seguintes abordagens:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="7fdaa-162">Instalar a extensão de site de visualização</span><span class="sxs-lookup"><span data-stu-id="7fdaa-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="7fdaa-163">Implantar o aplicativo autocontido</span><span class="sxs-lookup"><span data-stu-id="7fdaa-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="7fdaa-164">Usar o Docker com aplicativos Web para contêineres</span><span class="sxs-lookup"><span data-stu-id="7fdaa-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="7fdaa-165">Se você tiver problemas ao usar a extensão de site de visualização, abra um problema no [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="7fdaa-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="7fdaa-166">Instalar a extensão de site de visualização</span><span class="sxs-lookup"><span data-stu-id="7fdaa-166">Install the preview site extention</span></span>

* <span data-ttu-id="7fdaa-167">No portal do Azure, navegue até a folha Serviço de Aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="7fdaa-168">Digite "ex" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="7fdaa-169">Selecione **Extensões**.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-169">Select **Extensions**.</span></span>
* <span data-ttu-id="7fdaa-170">Selecione "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="7fdaa-170">Select "Add".</span></span>

![Folha do Azure App com etapas anteriores](index/_static/x1.png)

* <span data-ttu-id="7fdaa-172">Selecione **Extensões de tempo de execução do ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="7fdaa-173">Selecione **OK** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="7fdaa-174">Quando as operações de adição forem concluídas, a visualização mais recente do .NET Core 2.1 será instalada.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="7fdaa-175">Você pode verificar a instalação executando `dotnet --info` no console.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="7fdaa-176">Na folha de Serviço de Aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-176">From the App Service blade:</span></span>

* <span data-ttu-id="7fdaa-177">Digite "con" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="7fdaa-178">Selecione **Console**.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-178">Select **Console**.</span></span>
* <span data-ttu-id="7fdaa-179">Digite `dotnet --info` no console.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-179">Enter `dotnet --info` in the console.</span></span>

![Folha do Azure App com etapas anteriores](index/_static/cons.png)

<span data-ttu-id="7fdaa-181">A imagem anterior era atual no momento em que este texto foi escrito.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="7fdaa-182">Você pode ver uma versão diferente.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-182">You may see a different version.</span></span>

<span data-ttu-id="7fdaa-183">O `dotnet --info` exibe o caminho para a extensão de site em que a visualização foi instalada.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="7fdaa-184">Ele mostra que o aplicativo está em execução na extensão de site em vez do local padrão *ProgramFiles*.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="7fdaa-185">Se você vir *ProgramFiles*, reinicie o site e execute `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="7fdaa-186">Usar a extensão de site de visualização com um modelo do ARM</span><span class="sxs-lookup"><span data-stu-id="7fdaa-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="7fdaa-187">Se você estiver usando um modelo do ARM para criar e implantar aplicativos, poderá usar o tipo de recurso `siteextensions` para adicionar a extensão de site a um aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="7fdaa-188">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="7fdaa-189">Implantar o aplicativo autocontido</span><span class="sxs-lookup"><span data-stu-id="7fdaa-189">Deploy the app self contained</span></span>

<span data-ttu-id="7fdaa-190">Você pode implantar um [aplicativo autocontido](/dotnet/core/deploying/#self-contained-deployments-scd) que carrega o tempo de execução de visualização com ele quando está sendo implantado.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="7fdaa-191">Ao implantar um aplicativo autocontido:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="7fdaa-192">Não é necessário preparar o seu site.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="7fdaa-193">Exige que você publique seu aplicativo de forma diferente que ao implantar um aplicativo depois que o SDK está instalado no servidor.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="7fdaa-194">Aplicativos autocontidos são uma opção para todos os aplicativos do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="7fdaa-195">Usar o Docker com aplicativos Web para contêineres</span><span class="sxs-lookup"><span data-stu-id="7fdaa-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="7fdaa-196">O [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contém as imagens de visualização do Docker 2.1 mais recentes.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="7fdaa-197">Você pode usá-las como sua imagem de base e implantar em Aplicativos Web para Contêineres como faria normalmente.</span><span class="sxs-lookup"><span data-stu-id="7fdaa-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7fdaa-198">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7fdaa-198">Additional resources</span></span>

* [<span data-ttu-id="7fdaa-199">Visão geral de aplicativos Web (vídeo de visão geral com 5 minutos)</span><span class="sxs-lookup"><span data-stu-id="7fdaa-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="7fdaa-200">Serviço de Aplicativo do Azure: o melhor lugar para hospedar seus aplicativos .NET (vídeo de visão geral com 55 minutos)</span><span class="sxs-lookup"><span data-stu-id="7fdaa-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="7fdaa-201">Azure Friday: experiência de diagnóstico e solução de problemas do Serviço de Aplicativo do Azure (vídeo com 12 minutos)</span><span class="sxs-lookup"><span data-stu-id="7fdaa-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="7fdaa-202">Visão geral de diagnóstico do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="7fdaa-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="7fdaa-203">O Serviço de Aplicativo do Azure no Windows Server usa o [IIS (Serviços de Informações da Internet)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="7fdaa-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="7fdaa-204">Os tópicos a seguir estão relacionados com a tecnologia subjacente do IIS:</span><span class="sxs-lookup"><span data-stu-id="7fdaa-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="7fdaa-205">Hospedar o ASP.NET Core no Windows com o IIS</span><span class="sxs-lookup"><span data-stu-id="7fdaa-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="7fdaa-206">Introdução ao Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdaa-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="7fdaa-207">Referência de configuração do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdaa-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="7fdaa-208">Módulos do IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fdaa-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="7fdaa-209">Biblioteca Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="7fdaa-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
