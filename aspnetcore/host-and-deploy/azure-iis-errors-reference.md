---
title: Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core
author: guardrex
description: Distinga erros comuns ao hospedar aplicativos ASP.NET Core no Serviço de Aplicativos do Azure e no IIS.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277313"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="7d85f-103">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d85f-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="7d85f-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7d85f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7d85f-105">A lista a seguir não é uma lista completa de erros.</span><span class="sxs-lookup"><span data-stu-id="7d85f-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="7d85f-106">Se você encontrar um erro não listado aqui, [abra um novo problema](https://github.com/aspnet/Docs/issues/new) com instruções detalhadas para reproduzir o erro.</span><span class="sxs-lookup"><span data-stu-id="7d85f-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="7d85f-107">Colete as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="7d85f-107">Collect the following information:</span></span>

* <span data-ttu-id="7d85f-108">Comportamento do navegador</span><span class="sxs-lookup"><span data-stu-id="7d85f-108">Browser behavior</span></span>
* <span data-ttu-id="7d85f-109">Entradas do Log de Eventos do Aplicativo</span><span class="sxs-lookup"><span data-stu-id="7d85f-109">Application Event Log entries</span></span>
* <span data-ttu-id="7d85f-110">Entradas do log de stdout do Módulo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d85f-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="7d85f-111">Compare as informações para os erros comuns a seguir.</span><span class="sxs-lookup"><span data-stu-id="7d85f-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="7d85f-112">Se uma correspondência for encontrada, siga o aviso de solução de problemas.</span><span class="sxs-lookup"><span data-stu-id="7d85f-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="7d85f-113">O instalador não pode obter os Pacotes Redistribuíveis do VC++</span><span class="sxs-lookup"><span data-stu-id="7d85f-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="7d85f-114">**Exceção do Instalador:** 0x80072efd ou 0x80072f76 – erro não especificado</span><span class="sxs-lookup"><span data-stu-id="7d85f-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="7d85f-115">**Exceção do Log do Instalador&#8224;:** erro 0x80072efd ou 0x80072f76: falha ao executar o pacote EXE</span><span class="sxs-lookup"><span data-stu-id="7d85f-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="7d85f-116">&#8224;O log está localizado em C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="7d85f-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="7d85f-117">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-117">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-118">Se o sistema não tiver acesso à Internet durante a instalação do pacote de hospedagem, essa exceção ocorrerá quando o instalador for impedido de obter os *Pacotes Redistribuíveis do Microsoft Visual C++ 2015*.</span><span class="sxs-lookup"><span data-stu-id="7d85f-118">If the system doesn't have Internet access while installing the Hosting Bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="7d85f-119">Obtenha um instalador do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="7d85f-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="7d85f-120">Se o instalador falhar, o servidor poderá não receber o tempo de execução do .NET Core necessário para hospedar uma FDD (implantação dependente de estrutura).</span><span class="sxs-lookup"><span data-stu-id="7d85f-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="7d85f-121">Ao hospedar uma FDD, confirme se o tempo de execução está instalado em Programas &amp; Recursos.</span><span class="sxs-lookup"><span data-stu-id="7d85f-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="7d85f-122">Se necessário, obtenha um instalador de tempo de execução de [Todos os Downloads do .NET](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="7d85f-122">If needed, obtain a runtime installer from [.NET All Downloads](https://www.microsoft.com/net/download/all).</span></span> <span data-ttu-id="7d85f-123">Depois de instalar o tempo de execução, reinicie o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="7d85f-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="7d85f-124">O upgrade do sistema operacional removeu o Módulo do ASP.NET Core de 32 bits</span><span class="sxs-lookup"><span data-stu-id="7d85f-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="7d85f-125">**Log do Aplicativo:** a DLL do Módulo **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** falhou ao ser carregada.</span><span class="sxs-lookup"><span data-stu-id="7d85f-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="7d85f-126">Os dados são o erro.</span><span class="sxs-lookup"><span data-stu-id="7d85f-126">The data is the error.</span></span>

<span data-ttu-id="7d85f-127">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-127">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-128">Arquivos que não são do sistema operacional no diretório **C:\Windows\SysWOW64\inetsrv** não são preservados durante um upgrade do sistema operacional.</span><span class="sxs-lookup"><span data-stu-id="7d85f-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="7d85f-129">Se o Módulo do ASP.NET Core estiver instalado antes de uma atualização do sistema operacional e, em seguida, qualquer AppPool for executado no modo de 32 bits após uma atualização do sistema operacional, esse problema será encontrado.</span><span class="sxs-lookup"><span data-stu-id="7d85f-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="7d85f-130">Após um upgrade do sistema operacional, repare o Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d85f-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="7d85f-131">Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="7d85f-131">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="7d85f-132">Selecione **Reparar** ao executar o instalador.</span><span class="sxs-lookup"><span data-stu-id="7d85f-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="7d85f-133">Conflitos de plataforma com o RID</span><span class="sxs-lookup"><span data-stu-id="7d85f-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="7d85f-134">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="7d85f-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="7d85f-135">**Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\{PATH}\' falhou ao iniciar o processo com a linha de comando '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span><span class="sxs-lookup"><span data-stu-id="7d85f-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="7d85f-136">**Log do Módulo do ASP.NET Core:** exceção sem tratamento: System.BadImageFormatException: não foi possível carregar o arquivo ou o assembly '{assembly}.dll'.</span><span class="sxs-lookup"><span data-stu-id="7d85f-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="7d85f-137">Foi feita uma tentativa de carregar um programa com um formato incorreto.</span><span class="sxs-lookup"><span data-stu-id="7d85f-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="7d85f-138">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-138">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-139">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7d85f-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="7d85f-140">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="7d85f-141">Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="7d85f-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="7d85f-142">Confirme que o `<PlatformTarget>` no *.csproj* não entra em conflito com o RID.</span><span class="sxs-lookup"><span data-stu-id="7d85f-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="7d85f-143">Por exemplo, não especifique um `<PlatformTarget>` igual a `x86` e publique com um RID igual a `win10-x64`, usando *dotnet publish -c Release -r win10-x64* ou definindo o `<RuntimeIdentifiers>` no *.csproj* como `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="7d85f-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="7d85f-144">O projeto é publicado sem avisos nem erros, mas falha com as exceções registradas acima no sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="7d85f-145">Se essa exceção ocorrer para uma implantação dos Aplicativos do Azure ao fazer upgrade de um aplicativo e implantar assemblies mais recentes, exclua manualmente todos os arquivos da implantação anterior.</span><span class="sxs-lookup"><span data-stu-id="7d85f-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="7d85f-146">Assemblies incompatíveis remanescentes podem resultar em uma exceção `System.BadImageFormatException` durante a implantação de um aplicativo atualizado.</span><span class="sxs-lookup"><span data-stu-id="7d85f-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="7d85f-147">Ponto de extremidade de URI incorreto ou site interrompido</span><span class="sxs-lookup"><span data-stu-id="7d85f-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="7d85f-148">**Navegador:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="7d85f-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="7d85f-149">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="7d85f-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="7d85f-150">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="7d85f-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="7d85f-151">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-151">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-152">Confirme se que o ponto de extremidade do URI correto para o aplicativo está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="7d85f-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="7d85f-153">Verifique as associações.</span><span class="sxs-lookup"><span data-stu-id="7d85f-153">Check the bindings.</span></span>

* <span data-ttu-id="7d85f-154">Confirme que o site do IIS não está no estado *Parado*.</span><span class="sxs-lookup"><span data-stu-id="7d85f-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="7d85f-155">Recursos do servidor CoreWebEngine ou W3SVC desabilitados</span><span class="sxs-lookup"><span data-stu-id="7d85f-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="7d85f-156">**Exceção do Sistema Operacional:** os recursos CoreWebEngine e W3SVC do IIS 7.0 devem ser instalados para usar o Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d85f-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="7d85f-157">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-157">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-158">Confirme que a função e os recursos apropriados estão habilitados.</span><span class="sxs-lookup"><span data-stu-id="7d85f-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="7d85f-159">Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="7d85f-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="7d85f-160">Caminho físico do site incorreto ou aplicativo ausente</span><span class="sxs-lookup"><span data-stu-id="7d85f-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="7d85f-161">**Navegador:** 403 Proibido – acesso negado **OU** 403.14 Proibido – o servidor Web está configurado para não listar o conteúdo deste diretório.</span><span class="sxs-lookup"><span data-stu-id="7d85f-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="7d85f-162">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="7d85f-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="7d85f-163">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="7d85f-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="7d85f-164">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-164">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-165">Confira as **Configurações Básicas** no site do IIS e a pasta do aplicativo físico.</span><span class="sxs-lookup"><span data-stu-id="7d85f-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="7d85f-166">Confirme que o aplicativo está na pasta no **Caminho físico** do site do IIS.</span><span class="sxs-lookup"><span data-stu-id="7d85f-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="7d85f-167">Função incorreta, módulo não instalado ou permissões incorretas</span><span class="sxs-lookup"><span data-stu-id="7d85f-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="7d85f-168">**Navegador:** 500.19 Erro interno do servidor – a página solicitada não pode ser acessada porque os dados de configuração relacionados da página são inválidos.</span><span class="sxs-lookup"><span data-stu-id="7d85f-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="7d85f-169">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="7d85f-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="7d85f-170">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="7d85f-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="7d85f-171">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-171">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-172">Confirme que você habilitou a função apropriada.</span><span class="sxs-lookup"><span data-stu-id="7d85f-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="7d85f-173">Consulte [Configuração do IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="7d85f-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="7d85f-174">Verifique **Programas &amp; Recursos** e confirme se o **Módulo do Microsoft ASP.NET Core** foi instalado.</span><span class="sxs-lookup"><span data-stu-id="7d85f-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="7d85f-175">Se o **Módulo do Microsoft ASP.NET Core** não estiver presente na lista de programas instalados, instale o módulo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="7d85f-176">Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="7d85f-176">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="7d85f-177">Verifique se o **Pool de aplicativos** > **Modelo de processo** > **Identidade** está definido como **ApplicationPoolIdentity** ou se a identidade personalizada tem as permissões corretas para acessar a pasta de implantação do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="7d85f-178">processPath incorreto, variável de PATH ausente, pacote de hospedagem não instalado, sistema/IIS não reiniciado, Pacotes Redistribuíveis do VC++ não instalados ou violação de acesso de dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="7d85f-178">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="7d85f-179">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="7d85f-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="7d85f-180">**Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="7d85f-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="7d85f-181">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio</span><span class="sxs-lookup"><span data-stu-id="7d85f-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="7d85f-182">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-182">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-183">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7d85f-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="7d85f-184">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="7d85f-185">Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="7d85f-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="7d85f-186">Verifique o atributo *processPath* no elemento `<aspNetCore>` em *web.config* para confirmar se ele é *dotnet* para uma FDD (implantação dependente de estrutura) ou *.\{assembly}.exe* para uma SCD (implantação autossuficiente).</span><span class="sxs-lookup"><span data-stu-id="7d85f-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="7d85f-187">Para uma FDD, o *dotnet.exe* pode não estar acessível por meio das configurações de PATH.</span><span class="sxs-lookup"><span data-stu-id="7d85f-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="7d85f-188">Confirme se *C:\Program Files\dotnet\* existe nas configurações de PATH do Sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="7d85f-189">Para uma FDD, o *dotnet.exe* pode não estar acessível para a identidade do usuário do Pool de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="7d85f-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="7d85f-190">Confirme se a identidade do usuário do AppPool tem acesso ao diretório *C:\Program Files\dotnet*.</span><span class="sxs-lookup"><span data-stu-id="7d85f-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="7d85f-191">Confirme se não há nenhuma regra de negação configurada para a identidade do usuário do AppPool no *C:\Arquivos de Programas\dotnet* e nos diretórios do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="7d85f-192">Talvez você tenha implantado uma FDD e instalado o .NET Core sem reiniciar o IIS.</span><span class="sxs-lookup"><span data-stu-id="7d85f-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="7d85f-193">Reinicie o servidor ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="7d85f-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="7d85f-194">Você pode ter implantado uma FDD sem instalar o tempo de execução do .NET Core no sistema de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="7d85f-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="7d85f-195">Se o tempo de execução do .NET Core ainda não foi instalado, execute o **Instalador do Pacote de Hospedagem do .NET Core** no sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span> <span data-ttu-id="7d85f-196">Veja [Instalar o pacote de Hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="7d85f-196">See [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="7d85f-197">Se estiver tentando instalar o tempo de execução do .NET Core em um sistema sem uma conexão com a Internet, obtenha o tempo de execução em [Todos os Downloads do .NET](https://www.microsoft.com/net/download/all) e execute o instalador do pacote de hospedagem para instalar o Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d85f-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET All Downloads](https://www.microsoft.com/net/download/all) and run the Hosting Bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="7d85f-198">Conclua a instalação reiniciando o sistema ou o IIS executando **net stop was /y** seguido por **net start w3svc** em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="7d85f-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="7d85f-199">Talvez você tenha implantado uma FDD e os *Pacotes redistribuíveis do Microsoft Visual C++ 2015 (x64)* não estejam instalados no sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="7d85f-200">Obtenha um instalador do [Centro de Download da Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="7d85f-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="7d85f-201">Argumentos incorretos do elemento \<aspNetCore\></span><span class="sxs-lookup"><span data-stu-id="7d85f-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="7d85f-202">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="7d85f-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="7d85f-203">**Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="7d85f-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="7d85f-204">**Log do Módulo do ASP.NET Core:** o aplicativo a ser executado não existe: 'PATH\{assembly}.dll'</span><span class="sxs-lookup"><span data-stu-id="7d85f-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="7d85f-205">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-205">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-206">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7d85f-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="7d85f-207">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="7d85f-208">Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="7d85f-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="7d85f-209">Examine o atributo *arguments* no elemento `<aspNetCore>` no *web.config* para confirmar se ele: (a) é *.\{assembly}.dll* de uma FDD (implantação dependente de estrutura); ou (b) não está presente, é uma cadeia de caracteres vazia (*arguments=""*) ou uma lista de argumentos do aplicativo (*arguments="arg1, arg2, ..."*) para uma SCD (implantação autossuficiente).</span><span class="sxs-lookup"><span data-stu-id="7d85f-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="7d85f-210">Versão do .NET Framework ausente</span><span class="sxs-lookup"><span data-stu-id="7d85f-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="7d85f-211">**Navegador:** 502.3 Gateway incorreto – Erro de conexão ao tentar encaminhar a solicitação.</span><span class="sxs-lookup"><span data-stu-id="7d85f-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="7d85f-212">**Log do Aplicativo:** ErrorCode = O aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' falhou ao iniciar o processo com a linha de comando '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span><span class="sxs-lookup"><span data-stu-id="7d85f-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="7d85f-213">**Log do Módulo do ASP.NET Core:** exceção do método, arquivo ou assembly ausente.</span><span class="sxs-lookup"><span data-stu-id="7d85f-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="7d85f-214">O método, o arquivo ou o assembly especificado na exceção é um método, arquivo ou assembly do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7d85f-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="7d85f-215">Solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="7d85f-215">Troubleshooting:</span></span>

* <span data-ttu-id="7d85f-216">Instale a versão do .NET Framework ausente no sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="7d85f-217">Para uma FDD (implantação dependente de estrutura), confirme se você tem o tempo de execução correto instalado no sistema.</span><span class="sxs-lookup"><span data-stu-id="7d85f-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="7d85f-218">Se o projeto foi atualizado de 1.1 para 2.0 e implantado no sistema de hospedagem e resultou nessa exceção, verifique se a estrutura 2.0 está no sistema de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="7d85f-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="7d85f-219">Pool de aplicativos interrompido</span><span class="sxs-lookup"><span data-stu-id="7d85f-219">Stopped Application Pool</span></span>

* <span data-ttu-id="7d85f-220">**Navegador:** 503 Serviço não disponível</span><span class="sxs-lookup"><span data-stu-id="7d85f-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="7d85f-221">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="7d85f-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="7d85f-222">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="7d85f-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="7d85f-223">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7d85f-223">Troubleshooting</span></span>

* <span data-ttu-id="7d85f-224">Confirme que o Pool de Aplicativos não está no estado *Parado*.</span><span class="sxs-lookup"><span data-stu-id="7d85f-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="7d85f-225">Middleware de Integração do IIS não implementado</span><span class="sxs-lookup"><span data-stu-id="7d85f-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="7d85f-226">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="7d85f-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="7d85f-227">**Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' criou um processo com a linha de comando '"C:\\{PATH}\{assembly}.{exe|dll}"', mas falhou, não respondeu ou não escutou na porta '{PORT}' fornecida, ErrorCode = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="7d85f-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="7d85f-228">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal.</span><span class="sxs-lookup"><span data-stu-id="7d85f-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="7d85f-229">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7d85f-229">Troubleshooting</span></span>

* <span data-ttu-id="7d85f-230">Confirme se o aplicativo é executado localmente no Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7d85f-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="7d85f-231">Uma falha do processo pode ser o resultado de um problema no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="7d85f-232">Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="7d85f-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="7d85f-233">Confirme que uma das seguintes condições é verdadeira:</span><span class="sxs-lookup"><span data-stu-id="7d85f-233">Confirm that either:</span></span>
  * <span data-ttu-id="7d85f-234">O middleware de integração de IIS é referenciado chamando-se o método `UseIISIntegration` no `WebHostBuilder` do aplicativo (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="7d85f-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="7d85f-235">Os aplicativos usam o método `CreateDefaultBuilder` (ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="7d85f-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="7d85f-236">Veja [Hospedar no ASP.NET Core](xref:fundamentals/host/index) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="7d85f-236">See [Host in ASP.NET Core](xref:fundamentals/host/index) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="7d85f-237">O subaplicativo inclui uma seção \<manipuladores\></span><span class="sxs-lookup"><span data-stu-id="7d85f-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="7d85f-238">**Navegador:** 500.19 Erro HTTP – erro interno do servidor</span><span class="sxs-lookup"><span data-stu-id="7d85f-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="7d85f-239">**Log do Aplicativo:** nenhuma entrada</span><span class="sxs-lookup"><span data-stu-id="7d85f-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="7d85f-240">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mostrando uma operação normal do aplicativo raiz.</span><span class="sxs-lookup"><span data-stu-id="7d85f-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="7d85f-241">O arquivo de log não foi criado para o subaplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="7d85f-242">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7d85f-242">Troubleshooting</span></span>

* <span data-ttu-id="7d85f-243">Confirme se o arquivo *web.config* do subaplicativo não inclui uma seção `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="7d85f-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="7d85f-244">caminho do log de stdout incorreto</span><span class="sxs-lookup"><span data-stu-id="7d85f-244">stdout log path incorrect</span></span>

* <span data-ttu-id="7d85f-245">**Navegador:** o aplicativo responde normalmente.</span><span class="sxs-lookup"><span data-stu-id="7d85f-245">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="7d85f-246">**Log do Aplicativo:** Aviso: não foi possível criar o stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span><span class="sxs-lookup"><span data-stu-id="7d85f-246">**Application Log:** Warning: Could not create stdoutLogFile \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\path_doesnt_exist\stdout_8748_201831835937.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="7d85f-247">**Log do Módulo do ASP.NET Core:** arquivo de log não criado</span><span class="sxs-lookup"><span data-stu-id="7d85f-247">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="7d85f-248">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7d85f-248">Troubleshooting</span></span>

* <span data-ttu-id="7d85f-249">O caminho `stdoutLogFile` especificado no elemento `<aspNetCore>` de *web.config* não existe.</span><span class="sxs-lookup"><span data-stu-id="7d85f-249">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="7d85f-250">Para obter mais informações, veja a seção [Criação e redirecionamento de log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) do tópico de referência de configuração do Módulo do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d85f-250">For more information, see the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) section of the ASP.NET Core Module configuration reference topic.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="7d85f-251">Problema geral de configuração do aplicativo</span><span class="sxs-lookup"><span data-stu-id="7d85f-251">Application configuration general issue</span></span>

* <span data-ttu-id="7d85f-252">**Navegador:** 502.5 Erro HTTP – falha do processo</span><span class="sxs-lookup"><span data-stu-id="7d85f-252">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="7d85f-253">**Log do Aplicativo:** o aplicativo 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' com a raiz física 'C:\\{PATH}\' criou um processo com a linha de comando '"C:\\{PATH}\{assembly}.{exe|dll}"', mas falhou, não respondeu ou não escutou na porta '{PORT}' fornecida, ErrorCode = '0x800705b4'</span><span class="sxs-lookup"><span data-stu-id="7d85f-253">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="7d85f-254">**Log do Módulo do ASP.NET Core:** arquivo de log criado, mas vazio</span><span class="sxs-lookup"><span data-stu-id="7d85f-254">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="7d85f-255">Solução de problemas</span><span class="sxs-lookup"><span data-stu-id="7d85f-255">Troubleshooting</span></span>

* <span data-ttu-id="7d85f-256">Essa exceção geral indica que o processo não pôde ser iniciado, provavelmente, devido a um problema de configuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7d85f-256">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="7d85f-257">Consultando [Estrutura de diretório](xref:host-and-deploy/directory-structure), confirme se as pastas e os arquivos implantados do aplicativo são apropriados e se os arquivos de configuração do aplicativo estão presentes e contêm as configurações corretas para o aplicativo e o ambiente.</span><span class="sxs-lookup"><span data-stu-id="7d85f-257">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="7d85f-258">Para obter mais informações, confira [Solução de problemas](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="7d85f-258">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
