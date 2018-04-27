---
title: Referência de erros comuns para o serviço de aplicativo do Azure e o IIS com o ASP.NET Core
author: guardrex
description: Distingui erros comuns ao hospedar aplicativos ASP.NET Core no serviço de aplicativos do Azure e no IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 1500f026c245f80de4120d6db4901cb117552966
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="e75b6-103">Referência de erros comuns para o serviço de aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e75b6-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="e75b6-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e75b6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e75b6-105">A lista a seguir não é uma lista completa de erros.</span><span class="sxs-lookup"><span data-stu-id="e75b6-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="e75b6-106">Se você encontrar um erro não listado aqui, [abrir um novo problema](https://github.com/aspnet/Docs/issues/new) com instruções detalhadas para reproduzir o erro.</span><span class="sxs-lookup"><span data-stu-id="e75b6-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="e75b6-107">Colete as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="e75b6-107">Collect the following information:</span></span>

* <span data-ttu-id="e75b6-108">Comportamento do navegador</span><span class="sxs-lookup"><span data-stu-id="e75b6-108">Browser behavior</span></span>
* <span data-ttu-id="e75b6-109">Entradas de Log de eventos do aplicativo</span><span class="sxs-lookup"><span data-stu-id="e75b6-109">Application Event Log entries</span></span>
* <span data-ttu-id="e75b6-110">Entradas de log do ASP.NET Core módulo stdout</span><span class="sxs-lookup"><span data-stu-id="e75b6-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="e75b6-111">Compare as informações para os seguintes erros comuns.</span><span class="sxs-lookup"><span data-stu-id="e75b6-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="e75b6-112">Se uma correspondência for encontrada, siga os avisos de solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="e75b6-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="e75b6-113">O instalador não pode obter os Pacotes Redistribuíveis do VC++</span><span class="sxs-lookup"><span data-stu-id="e75b6-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="e75b6-114">**Exceção do Instalador:** 0x80072efd ou 0x80072f76 – erro não especificado</span><span class="sxs-lookup"><span data-stu-id="e75b6-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="e75b6-115">**Exceção do Log do Instalador&#8224;:** erro 0x80072efd ou 0x80072f76: falha ao executar o pacote EXE</span><span class="sxs-lookup"><span data-stu-id="e75b6-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="e75b6-116">&#8224;O log está localizado em C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="e75b6-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="e75b6-117">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-117">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-118">Se o sistema não tiver acesso à Internet durante a instalação do pacote de hospedagem, essa exceção ocorre quando o instalador não poderão obter o *Microsoft Visual C++ 2015 redistribuível*.</span><span class="sxs-lookup"><span data-stu-id="e75b6-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="e75b6-119">Obter um instalador do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="e75b6-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="e75b6-120">Se o instalador falhar, o servidor pode não receber o tempo de execução do .NET Core necessário para hospedar uma implantação de framework dependente (FDD).</span><span class="sxs-lookup"><span data-stu-id="e75b6-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="e75b6-121">Se hospedando um FDD, confirme que o tempo de execução é instalado em programas &amp; recursos.</span><span class="sxs-lookup"><span data-stu-id="e75b6-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="e75b6-122">Se necessário, obtenha um instalador de tempo de execução de [.NET todos os Downloads](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e75b6-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="e75b6-123">Depois de instalar o tempo de execução, reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="e75b6-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="e75b6-124">O upgrade do sistema operacional removeu o Módulo do ASP.NET Core de 32 bits</span><span class="sxs-lookup"><span data-stu-id="e75b6-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="e75b6-125">**Log do Aplicativo:** a DLL do Módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** falhou ao ser carregada.</span><span class="sxs-lookup"><span data-stu-id="e75b6-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="e75b6-126">Os dados são o erro.</span><span class="sxs-lookup"><span data-stu-id="e75b6-126">The data is the error.</span></span>

<span data-ttu-id="e75b6-127">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-127">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-128">Arquivos que não são do sistema operacional no diretório **C:\Windows\SysWOW64\inetsrv** não são preservados durante um upgrade do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="e75b6-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="e75b6-129">Se o módulo de núcleo do ASP.NET está instalado antes de uma atualização do sistema operacional e, em seguida, qualquer AppPool é executado no modo de 32 bits depois de uma atualização do sistema operacional, esse problema é encontrado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="e75b6-130">Após um upgrade do sistema operacional, repare o Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e75b6-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="e75b6-131">Consulte [instalar o pacote de hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e75b6-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="e75b6-132">Selecione **reparo** quando o instalador é executado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="e75b6-133">Conflitos de plataforma com o RID</span><span class="sxs-lookup"><span data-stu-id="e75b6-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="e75b6-134">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="e75b6-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e75b6-135">**Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\{caminho}\' Falha ao iniciar o processo com linha de comando ' "c:\\{caminho} {assembly}. { exe | dll} "', código de erro = ' 0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="e75b6-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="e75b6-136">**Log de módulo do ASP.NET Core:** exceção sem tratamento: System. BadImageFormatException: não foi possível carregar arquivo ou assembly '{assembly}. dll'.</span><span class="sxs-lookup"><span data-stu-id="e75b6-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="e75b6-137">Foi feita uma tentativa de carregar um programa com um formato incorreto.</span><span class="sxs-lookup"><span data-stu-id="e75b6-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="e75b6-138">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-138">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-139">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e75b6-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e75b6-140">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e75b6-141">Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e75b6-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e75b6-142">Confirme se o `<PlatformTarget>` no *. csproj* não entra em conflito com o RID.</span><span class="sxs-lookup"><span data-stu-id="e75b6-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="e75b6-143">Por exemplo, não especifique um `<PlatformTarget>` de `x86` e publicar com uma RID da `win10-x64`, usando *dotnet publicar - c - r de versão para win10-x64* ou definindo o `<RuntimeIdentifiers>` no *. csproj*  para `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="e75b6-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="e75b6-144">O projeto é publicado sem avisos nem erros, mas falha com as exceções registradas acima no sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="e75b6-145">Se essa exceção ocorre para uma implantação de aplicativos do Azure ao atualizar um aplicativo e implantando assemblies mais recentes, exclua manualmente todos os arquivos de implantação anterior.</span><span class="sxs-lookup"><span data-stu-id="e75b6-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="e75b6-146">Assemblies incompatíveis remanescentes podem resultar em uma exceção `System.BadImageFormatException` durante a implantação de um aplicativo atualizado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="e75b6-147">Ponto de extremidade de URI incorreto ou site interrompido</span><span class="sxs-lookup"><span data-stu-id="e75b6-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="e75b6-148">**Navegador:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="e75b6-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="e75b6-149">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="e75b6-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="e75b6-150">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="e75b6-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e75b6-151">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-151">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-152">Confirme se que o ponto de extremidade URI correto para o aplicativo está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="e75b6-153">Verifique as associações.</span><span class="sxs-lookup"><span data-stu-id="e75b6-153">Check the bindings.</span></span>

* <span data-ttu-id="e75b6-154">Confirme que o site do IIS não está no *parado* estado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="e75b6-155">Recursos do servidor CoreWebEngine ou W3SVC desabilitados</span><span class="sxs-lookup"><span data-stu-id="e75b6-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="e75b6-156">**Exceção do Sistema Operacional:** os recursos CoreWebEngine e W3SVC do IIS 7.0 devem ser instalados para usar o Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e75b6-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="e75b6-157">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-157">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-158">Confirme se a função adequada e os recursos estão habilitados.</span><span class="sxs-lookup"><span data-stu-id="e75b6-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="e75b6-159">Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e75b6-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="e75b6-160">Caminho físico do site incorreto ou ausente do aplicativo</span><span class="sxs-lookup"><span data-stu-id="e75b6-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="e75b6-161">**Navegador:** 403 Proibido – acesso negado **OU** 403.14 Proibido – o servidor Web está configurado para não listar o conteúdo deste diretório.</span><span class="sxs-lookup"><span data-stu-id="e75b6-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="e75b6-162">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="e75b6-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="e75b6-163">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="e75b6-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e75b6-164">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-164">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-165">Verifique o site do IIS **configurações básicas** e a pasta física do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="e75b6-166">Confirme se o aplicativo está na pasta em que o site do IIS **caminho físico**.</span><span class="sxs-lookup"><span data-stu-id="e75b6-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="e75b6-167">Função incorreta, módulo não instalado ou permissões incorretas</span><span class="sxs-lookup"><span data-stu-id="e75b6-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="e75b6-168">**Navegador:** 500.19 Erro interno do servidor – a página solicitada não pode ser acessada porque os dados de configuração relacionados da página são inválidos.</span><span class="sxs-lookup"><span data-stu-id="e75b6-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="e75b6-169">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="e75b6-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="e75b6-170">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="e75b6-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e75b6-171">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-171">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-172">Confirme que a função adequada está habilitada.</span><span class="sxs-lookup"><span data-stu-id="e75b6-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="e75b6-173">Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="e75b6-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="e75b6-174">Verifique **Programas &amp; Recursos** e confirme se o **Módulo do Microsoft ASP.NET Core** foi instalado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="e75b6-175">Se o **Módulo do Microsoft ASP.NET Core** não estiver presente na lista de programas instalados, instale o módulo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="e75b6-176">Consulte [instalar o .NET Core hospedagem pacote](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e75b6-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="e75b6-177">Verifique se o **Pool de aplicativos** > **modelo de processo** > **identidade** é definido como **ApplicationPoolIdentity** ou a identidade personalizada tem as permissões corretas para acessar a pasta de implantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="e75b6-178">ProcessPath incorreto, variável de caminho ausente, o pacote de hospedagem não instalado, não reiniciado sistema/IIS, VC + + redistribuível não está instalado ou dotnet.exe violação de acesso</span><span class="sxs-lookup"><span data-stu-id="e75b6-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="e75b6-179">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="e75b6-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e75b6-180">**Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' ".\{ assembly} .exe"', código de erro = ' 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="e75b6-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="e75b6-181">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio</span><span class="sxs-lookup"><span data-stu-id="e75b6-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e75b6-182">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-182">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-183">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e75b6-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e75b6-184">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e75b6-185">Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e75b6-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e75b6-186">Verifique o *processPath* atributo no `<aspNetCore>` elemento *Web. config* para confirmar se ele está *dotnet* para uma implantação do framework dependente (FDD) ou *. \{assembly} .exe* para uma implantação autossuficiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="e75b6-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="e75b6-187">Para uma FDD, o *dotnet.exe* pode não estar acessível por meio das configurações de PATH.</span><span class="sxs-lookup"><span data-stu-id="e75b6-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="e75b6-188">Confirme se *C:\Program Files\dotnet\* existe nas configurações de PATH do Sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="e75b6-189">Para uma FDD, o *dotnet.exe* pode não estar acessível para a identidade do usuário do Pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="e75b6-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="e75b6-190">Confirme se a identidade do usuário do AppPool tem acesso ao diretório *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="e75b6-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="e75b6-191">Confirme que não há nenhuma regra de negação configurada para a identidade do usuário AppPool no *Files\dotnet C:\Program* e diretórios do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="e75b6-192">Um FDD pode ter sido implantado e .NET Core instalado sem a reinicialização do IIS.</span><span class="sxs-lookup"><span data-stu-id="e75b6-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="e75b6-193">Reinicie o servidor ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="e75b6-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e75b6-194">Um FDD pode ter sido implantado sem instalar o tempo de execução do .NET Core no sistema de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="e75b6-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="e75b6-195">Se o tempo de execução do .NET Core ainda não foi instalado, execute o **instalador do pacote de hospedagem do .NET Core** no sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="e75b6-196">Consulte [instalar o .NET Core hospedagem pacote](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="e75b6-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="e75b6-197">Se você tentar instalar o runtime do .NET Core em um sistema sem uma conexão de Internet, obter o tempo de execução de [.NET todos os Downloads](https://www.microsoft.com/net/download/all) e execute o instalador do pacote de hospedagem para instalar o módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e75b6-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="e75b6-198">Conclua a instalação reiniciando o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="e75b6-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e75b6-199">Um FDD pode ter sido implantado e o *Microsoft Visual C++ 2015 redistribuível (x64)* não está instalado no sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="e75b6-200">Obter um instalador do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="e75b6-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="e75b6-201">Argumentos incorretos do elemento \<aspNetCore\></span><span class="sxs-lookup"><span data-stu-id="e75b6-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="e75b6-202">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="e75b6-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e75b6-203">**Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' "dotnet".\{ . dll de assembly}', código de erro = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="e75b6-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e75b6-204">**Log de módulo do ASP.NET Core:** a execução do aplicativo não existe: ' caminho\{assembly}. dll '</span><span class="sxs-lookup"><span data-stu-id="e75b6-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="e75b6-205">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-205">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-206">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e75b6-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e75b6-207">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e75b6-208">Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e75b6-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e75b6-209">Examine o *argumentos* atributo no `<aspNetCore>` elemento *Web. config* para confirmar se ele é (a) *.\{ . dll de assembly}* para uma implantação de framework dependente (FDD); ou (b) não está presente, uma cadeia de caracteres vazia (*argumentos = ""*), ou uma lista de argumentos do aplicativo (*argumentos = "arg1, arg2,..."*) para uma implantação autossuficiente (SCD).</span><span class="sxs-lookup"><span data-stu-id="e75b6-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="e75b6-210">Versão do .NET Framework ausente</span><span class="sxs-lookup"><span data-stu-id="e75b6-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="e75b6-211">**Navegador:** 502.3 Gateway incorreto – Erro de conexão ao tentar encaminhar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="e75b6-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="e75b6-212">**Log de aplicativo:** ErrorCode = aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' Falha ao iniciar o processo com linha de comando ' "dotnet".\{ . dll de assembly}', código de erro = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="e75b6-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e75b6-213">**Log do Módulo do ASP.NET Core:** exceção do método, arquivo ou assembly ausente.</span><span class="sxs-lookup"><span data-stu-id="e75b6-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="e75b6-214">O método, o arquivo ou o assembly especificado na exceção é um método, arquivo ou assembly do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e75b6-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="e75b6-215">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="e75b6-215">Troubleshooting:</span></span>

* <span data-ttu-id="e75b6-216">Instale a versão do .NET Framework ausente no sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="e75b6-217">Para uma implantação do framework dependente (FDD), confirme que o tempo de execução correto instalada no sistema.</span><span class="sxs-lookup"><span data-stu-id="e75b6-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="e75b6-218">Se o projeto é atualizado de 1.1 para 2.0, o sistema host, implantados e os resultados dessa exceção, verifique se o framework 2.0 está no sistema de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="e75b6-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="e75b6-219">Pool de aplicativos interrompido</span><span class="sxs-lookup"><span data-stu-id="e75b6-219">Stopped Application Pool</span></span>

* <span data-ttu-id="e75b6-220">**Navegador:** 503 Serviço não disponível</span><span class="sxs-lookup"><span data-stu-id="e75b6-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="e75b6-221">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="e75b6-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="e75b6-222">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="e75b6-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e75b6-223">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e75b6-223">Troubleshooting</span></span>

* <span data-ttu-id="e75b6-224">Confirme que o Pool de aplicativos não está no *parado* estado.</span><span class="sxs-lookup"><span data-stu-id="e75b6-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="e75b6-225">Middleware de Integração do IIS não implementado</span><span class="sxs-lookup"><span data-stu-id="e75b6-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="e75b6-226">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="e75b6-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e75b6-227">**Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' criou um processo com linha de comando ' "c:\\{caminho}\{assembly}. { exe | dll} "' mas o falhou ou foi resposta não ou não escutar na porta determinada '{PORT}', código de erro = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="e75b6-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e75b6-228">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal.</span><span class="sxs-lookup"><span data-stu-id="e75b6-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="e75b6-229">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e75b6-229">Troubleshooting</span></span>

* <span data-ttu-id="e75b6-230">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e75b6-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e75b6-231">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e75b6-232">Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e75b6-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="e75b6-233">Confirme se ou:</span><span class="sxs-lookup"><span data-stu-id="e75b6-233">Confirm that either:</span></span>
  * <span data-ttu-id="e75b6-234">O middleware de integração de IIS for referencedby chamando o `UseIISIntegration` método do aplicativo `WebHostBuilder` (ASP.NET Core 1. x)</span><span class="sxs-lookup"><span data-stu-id="e75b6-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="e75b6-235">As aplicativos usa o `CreateDefaultBuilder` método (ASP.NET Core 2. x).</span><span class="sxs-lookup"><span data-stu-id="e75b6-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="e75b6-236">Consulte [Hospedando no ASP.NET Core](xref:fundamentals/hosting) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="e75b6-236">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="e75b6-237">O subaplicativo inclui uma seção \<manipuladores\></span><span class="sxs-lookup"><span data-stu-id="e75b6-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="e75b6-238">**Navegador:** 500.19 Erro HTTP – erro interno do servidor</span><span class="sxs-lookup"><span data-stu-id="e75b6-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="e75b6-239">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="e75b6-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="e75b6-240">**Log de módulo do ASP.NET Core:** arquivo de Log criado e mostra uma operação normal para o aplicativo raiz.</span><span class="sxs-lookup"><span data-stu-id="e75b6-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="e75b6-241">Arquivo de log não foi criado para o aplicativo sub.</span><span class="sxs-lookup"><span data-stu-id="e75b6-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="e75b6-242">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e75b6-242">Troubleshooting</span></span>

* <span data-ttu-id="e75b6-243">Confirme se o arquivo *web.config* do subaplicativo não inclui uma seção `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="e75b6-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="e75b6-244">caminho do log stdout incorreto</span><span class="sxs-lookup"><span data-stu-id="e75b6-244">stdout log path incorrect</span></span>

* <span data-ttu-id="e75b6-245">**Navegador:** o aplicativo normalmente responde.</span><span class="sxs-lookup"><span data-stu-id="e75b6-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="e75b6-246">**Log de aplicativo:** Aviso: não foi possível criar o stdoutLogFile \\? \C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = - 2147024893.</span><span class="sxs-lookup"><span data-stu-id="e75b6-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="e75b6-247">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="e75b6-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e75b6-248">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e75b6-248">Troubleshooting</span></span>

* <span data-ttu-id="e75b6-249">O `stdoutLogFile` caminho especificado no `<aspNetCore>` elemento *Web. config* não existe.</span><span class="sxs-lookup"><span data-stu-id="e75b6-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="e75b6-250">Para obter mais informações, consulte o [criação e redirecionamento de Log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) seção do tópico de referência de configuração ASP.NET Core módulo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="e75b6-251">Problema geral de configuração do aplicativo</span><span class="sxs-lookup"><span data-stu-id="e75b6-251">Application configuration general issue</span></span>

* <span data-ttu-id="e75b6-252">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="e75b6-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e75b6-253">**Log de aplicativo:** aplicativo ' MACHINE/WEBROOT/APPHOST / {ASSEMBLY}' com raiz física ' c:\\{caminho}\' criou um processo com linha de comando ' "c:\\{caminho}\{assembly}. { exe | dll} "' mas o falhou ou foi resposta não ou não escutar na porta determinada '{PORT}', código de erro = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="e75b6-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e75b6-254">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio</span><span class="sxs-lookup"><span data-stu-id="e75b6-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e75b6-255">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="e75b6-255">Troubleshooting</span></span>

* <span data-ttu-id="e75b6-256">Essa exceção geral indica que o processo não pôde iniciar, provavelmente devido a um problema de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e75b6-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="e75b6-257">Referindo-se a [estrutura de diretórios](xref:host-and-deploy/directory-structure), confirme que o aplicativo implantado arquivos e pastas são apropriadas e que os arquivos de configuração do aplicativo estão presentes e conter as configurações corretas para o aplicativo e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="e75b6-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="e75b6-258">Para obter mais informações, consulte [solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="e75b6-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
