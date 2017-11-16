---
title: ASP.NET Core no Nano Server
author: shirhatti
description: "Saiba como executar um aplicativo ASP.NET Core existente e implantá-lo em uma instância do Nano Server executando IIS."
keywords: ASP.NET Core, nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: 337cc69ef522452c17cdd6ea4a5e71cd122035dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="692d8-104">ASP.NET Core com IIS no Nano Server</span><span class="sxs-lookup"><span data-stu-id="692d8-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="692d8-105">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="692d8-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="692d8-106">Neste tutorial, você executará um aplicativo ASP.NET Core existente e o implantará em uma instância do Nano Server executando IIS.</span><span class="sxs-lookup"><span data-stu-id="692d8-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="692d8-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="692d8-107">Introduction</span></span>

<span data-ttu-id="692d8-108">O Nano Server é uma opção de instalação no Windows Server 2016, oferecendo um volume pequeno, melhor segurança e melhor manutenção do que o Server Core ou o servidor completo.</span><span class="sxs-lookup"><span data-stu-id="692d8-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="692d8-109">Consulte a [documentação do Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) oficial para obter mais detalhes e links de download para versões de avaliação de 180 dias.</span><span class="sxs-lookup"><span data-stu-id="692d8-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="692d8-110">Há três maneiras para você experimentar o Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="692d8-111">Quando você entra com sua conta da MS:</span><span class="sxs-lookup"><span data-stu-id="692d8-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="692d8-112">Você pode baixar o arquivo ISO do Windows Server 2016 e criar uma imagem do Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="692d8-113">Baixe o VHD do Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="692d8-114">Crie uma VM no Azure usando a imagem do Nano Server na Galeria do Azure.</span><span class="sxs-lookup"><span data-stu-id="692d8-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="692d8-115">Se não tiver uma conta do Azure, você poderá obter uma versão de avaliação gratuita de 30 dias.</span><span class="sxs-lookup"><span data-stu-id="692d8-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="692d8-116">Neste tutorial estaremos usando a opção 2, o VHD do Nano Server pré-compilado do Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="692d8-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="692d8-117">Antes de continuar com este tutorial, você precisará da [saída publicada](xref:hosting/directory-structure) de um aplicativo ASP.NET Core existente.</span><span class="sxs-lookup"><span data-stu-id="692d8-117">Before proceeding with this tutorial, you will need the [published output](xref:hosting/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="692d8-118">Certifique-se de que seu aplicativo é criado para ser executado em um processo de **64 bits**.</span><span class="sxs-lookup"><span data-stu-id="692d8-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="692d8-119">Configurando a instância do Nano Server</span><span class="sxs-lookup"><span data-stu-id="692d8-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="692d8-120">[Criar uma nova Máquina Virtual usando o Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) no computador de desenvolvimento usando o VHD baixado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="692d8-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="692d8-121">O computador exigirá que você defina uma senha de administrador antes de fazer logon.</span><span class="sxs-lookup"><span data-stu-id="692d8-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="692d8-122">No console da VM, pressione F11 para definir a senha antes do primeiro logon.</span><span class="sxs-lookup"><span data-stu-id="692d8-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="692d8-123">Você também precisa verificar o endereço IP da nova VM, seja pela verificação do IP fixo do seu servidor DHCP fornecido ao provisionar a VM ou então nas configurações de rede do console de recuperação do Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="692d8-124">Vamos supor que a nova VM é executada com o endereço IP V4 local 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="692d8-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="692d8-125">Agora, você é capaz de gerenciá-lo usando a comunicação remota do PowerShell, que é a única maneira de administrar completamente o Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="692d8-126">Conectar-se à instância do Nano Server usando a Comunicação remota do PowerShell</span><span class="sxs-lookup"><span data-stu-id="692d8-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="692d8-127">Abra uma janela elevada do PowerShell para adicionar a instância remota do Nano Server à lista `TrustedHosts`.</span><span class="sxs-lookup"><span data-stu-id="692d8-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="692d8-128">Substitua a variável `$nanoServerIpAddress` com o endereço IP correto.</span><span class="sxs-lookup"><span data-stu-id="692d8-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="692d8-129">Depois de adicionar a instância do Nano Server a `TrustedHosts`, você pode se conectar a ela usando a Comunicação remota do PowerShell.</span><span class="sxs-lookup"><span data-stu-id="692d8-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="692d8-130">Uma conexão bem-sucedida resulta em um prompt com um formato parecido com: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="692d8-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="692d8-131">Criar um compartilhamento de arquivos</span><span class="sxs-lookup"><span data-stu-id="692d8-131">Creating a file share</span></span>

<span data-ttu-id="692d8-132">Crie um compartilhamento de arquivos no Nano server para que o aplicativo publicado possa ser copiado para ele.</span><span class="sxs-lookup"><span data-stu-id="692d8-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="692d8-133">Execute os comandos a seguir na sessão remota:</span><span class="sxs-lookup"><span data-stu-id="692d8-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="692d8-134">Depois de executar os comandos acima, você deve ser capaz de acessar este compartilhamento visitando `\\192.168.1.10\AspNetCoreSampleForNano` no Windows Explorer do computador host.</span><span class="sxs-lookup"><span data-stu-id="692d8-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="692d8-135">Abra a porta no firewall</span><span class="sxs-lookup"><span data-stu-id="692d8-135">Open port in the firewall</span></span>

<span data-ttu-id="692d8-136">Execute os comandos a seguir na sessão remota para abrir uma porta no firewall para permitir que o IIS escute tráfego TCP na porta TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="692d8-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="692d8-137">Instalando o IIS</span><span class="sxs-lookup"><span data-stu-id="692d8-137">Installing IIS</span></span>

<span data-ttu-id="692d8-138">Adicionar o provedor `NanoServerPackage` da Galeria do PowerShell.</span><span class="sxs-lookup"><span data-stu-id="692d8-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="692d8-139">Depois que o provedor está instalado e importado, você pode instalar pacotes do Windows.</span><span class="sxs-lookup"><span data-stu-id="692d8-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="692d8-140">Execute os comandos a seguir na sessão do PowerShell que foi criada anteriormente:</span><span class="sxs-lookup"><span data-stu-id="692d8-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="692d8-141">Para verificar rapidamente se o IIS está configurado corretamente, você pode visitar a URL `http://192.168.1.10/` e deverá ver uma página inicial.</span><span class="sxs-lookup"><span data-stu-id="692d8-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="692d8-142">Quando o IIS é instalado, um site chamado `Default Web Site` escutando na porta 80 é criado por padrão.</span><span class="sxs-lookup"><span data-stu-id="692d8-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="692d8-143">Instalando o módulo ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="692d8-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="692d8-144">O módulo do ASP.NET Core é um módulo IIS 7.5+ responsável pelo gerenciamento de processos de ouvintes HTTP do ASP.NET Core e solicitações de proxy para processos que ele gerencia.</span><span class="sxs-lookup"><span data-stu-id="692d8-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="692d8-145">No momento, o processo para instalar o módulo do ASP.NET Core para IIS é manual.</span><span class="sxs-lookup"><span data-stu-id="692d8-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="692d8-146">Você precisará instalar o [pacote de hospedagem do Windows Server do .NET Core](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) em um computador normal (não Nano).</span><span class="sxs-lookup"><span data-stu-id="692d8-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="692d8-147">Depois de instalar o pacote em um computador normal, você precisará copiar os arquivos a seguir para o compartilhamento de arquivos que criamos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="692d8-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="692d8-148">Em um servidor normal (não Nano) com o IIS, execute os seguintes comandos de cópia:</span><span class="sxs-lookup"><span data-stu-id="692d8-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="692d8-149">Substitua `C:\windows\system32\inetsrv` com `C:\Program Files\IIS Express` em um computador com Windows 10.</span><span class="sxs-lookup"><span data-stu-id="692d8-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="692d8-150">No lado do Nano, você precisará copiar os seguintes arquivos do compartilhamento de arquivos que criamos anteriormente para os locais válidos.</span><span class="sxs-lookup"><span data-stu-id="692d8-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="692d8-151">Portanto, execute os seguintes comandos de cópia:</span><span class="sxs-lookup"><span data-stu-id="692d8-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="692d8-152">Execute o seguinte script na sessão remota:</span><span class="sxs-lookup"><span data-stu-id="692d8-152">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="692d8-153">Exclua os arquivos *aspnetcore.dll* e *aspnetcore_schema.xml* do compartilhamento após a etapa acima.</span><span class="sxs-lookup"><span data-stu-id="692d8-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="692d8-154">Instalando o .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="692d8-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="692d8-155">Se seu aplicativo é publicado como uma [FDD (implantação dependente de estrutura)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), o .NET Core deve ser instalado no servidor.</span><span class="sxs-lookup"><span data-stu-id="692d8-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="692d8-156">Use o [script do PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) em uma sessão remota do PowerShell para instalar o .NET Framework em seu Nano Server.</span><span class="sxs-lookup"><span data-stu-id="692d8-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="692d8-157">Passe a versão do CLI com a opção `-Version`:</span><span class="sxs-lookup"><span data-stu-id="692d8-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="692d8-158">Publicando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="692d8-158">Publishing the application</span></span>

<span data-ttu-id="692d8-159">Copie a saída publicada do seu aplicativo existente para a raiz do compartilhamento de arquivos.</span><span class="sxs-lookup"><span data-stu-id="692d8-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="692d8-160">Talvez seja necessário fazer alterações no seu *web.config* para apontar o local para o qual você extraiu *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="692d8-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="692d8-161">Como alternativa, você pode adicionar *dotnet.exe* ao seu caminho.</span><span class="sxs-lookup"><span data-stu-id="692d8-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="692d8-162">Exemplo de como pode ser a aparência de um *web.config* se *dotnet.exe* **não** está no caminho:</span><span class="sxs-lookup"><span data-stu-id="692d8-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="692d8-163">Execute os comandos a seguir na sessão remota para criar um novo site no IIS para o aplicativo publicado em uma porta diferente da usada no site padrão.</span><span class="sxs-lookup"><span data-stu-id="692d8-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="692d8-164">Você também precisa abrir aquela porta para acessar a Web.</span><span class="sxs-lookup"><span data-stu-id="692d8-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="692d8-165">Esse script usa a `DefaultAppPool` para manter a simplicidade.</span><span class="sxs-lookup"><span data-stu-id="692d8-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="692d8-166">Para obter mais considerações sobre a execução em um pool de aplicativos, consulte [Pools de aplicativos](xref:publishing/iis#application-pools).</span><span class="sxs-lookup"><span data-stu-id="692d8-166">For more considerations on running under an application pool, see [Application Pools](xref:publishing/iis#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="692d8-167">Problema conhecido executando a CLI do .NET Core no Nano Server e solução alternativa</span><span class="sxs-lookup"><span data-stu-id="692d8-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="692d8-168">Executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="692d8-168">Running the application</span></span>

<span data-ttu-id="692d8-169">O aplicativo Web publicado é acessível em um navegador em `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="692d8-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="692d8-170">Se você tiver configurado o log conforme descrito em [Criação e redirecionamento de Log](xref:hosting/aspnet-core-module#log-creation-and-redirection), você poderá exibir os logs em *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="692d8-170">If you've set up logging as described in [Log creation and redirection](xref:hosting/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
