---
title: ASP.NET Core no Nano Server
author: shirhatti
description: Saiba como executar um aplicativo ASP.NET Core existente e implantá-lo em uma instância do Nano Server executando IIS.
manager: wpickett
ms.author: riande
ms.date: 11/04/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/nano-server
ms.openlocfilehash: 3cfe85e4591b99e3d5413a5532c5101d4a4810e2
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2018
---
# <a name="aspnet-core-on-nano-server"></a>ASP.NET Core no Nano Server

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Neste tutorial, você executará um aplicativo ASP.NET Core existente e o implantará em uma instância do Nano Server executando IIS.

## <a name="introduction"></a>Introdução

O Nano Server é uma opção de instalação no Windows Server 2016, oferecendo um volume pequeno, melhor segurança e melhor manutenção do que o Server Core ou o servidor completo. Consulte a [documentação do Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) oficial para obter mais detalhes e links de download para versões de avaliação de 180 dias. 

Há três maneiras para você experimentar o Nano Server. Quando você entra com sua conta da MS:

1. Você pode baixar o arquivo ISO do Windows Server 2016 e criar uma imagem do Nano Server.

2. Baixe o VHD do Nano Server.

3. Crie uma VM no Azure usando a imagem do Nano Server na Galeria do Azure. Uma avaliação gratuita do Azure está disponível.

Neste tutorial estaremos usando a opção 2, o VHD do Nano Server pré-compilado do Windows Server 2016.

Antes de continuar com este tutorial, você precisará da [saída publicada](xref:host-and-deploy/directory-structure) de um aplicativo ASP.NET Core existente. Certifique-se de que seu aplicativo é criado para ser executado em um processo de **64 bits**.

## <a name="setting-up-the-nano-server-instance"></a>Configurando a instância do Nano Server

[Criar uma nova Máquina Virtual usando o Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) no computador de desenvolvimento usando o VHD baixado anteriormente. O computador exigirá que você defina uma senha de administrador antes de fazer logon. No console da VM, pressione F11 para definir a senha antes do primeiro logon. Você também precisa verificar o endereço IP da nova VM, seja pela verificação do IP fixo do seu servidor DHCP fornecido ao provisionar a VM ou então nas configurações de rede do console de recuperação do Nano Server.

> [!NOTE]
> Vamos supor que a nova VM é executada com o endereço IP V4 local 192.168.1.10.

Agora, você é capaz de gerenciá-lo usando a comunicação remota do PowerShell, que é a única maneira de administrar completamente o Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Conectar-se à instância do Nano Server usando a Comunicação remota do PowerShell

Abra uma janela elevada do PowerShell para adicionar a instância remota do Nano Server à lista `TrustedHosts`.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Substitua a variável `$nanoServerIpAddress` com o endereço IP correto.

Depois de adicionar a instância do Nano Server a `TrustedHosts`, você pode se conectar a ela usando a Comunicação remota do PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

Uma conexão bem-sucedida resulta em um prompt com um formato parecido com: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>Criar um compartilhamento de arquivos

Crie um compartilhamento de arquivos no Nano server para que o aplicativo publicado possa ser copiado para ele. Execute os comandos a seguir na sessão remota:

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

Depois de executar os comandos acima, você deve ser capaz de acessar este compartilhamento visitando `\\192.168.1.10\AspNetCoreSampleForNano` no Windows Explorer do computador host.

## <a name="open-port-in-the-firewall"></a>Abra a porta no firewall

Execute os comandos a seguir na sessão remota para abrir uma porta no firewall para permitir que o IIS escute tráfego TCP na porta TCP/8000.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Instalando o IIS

Adicionar o provedor `NanoServerPackage` da Galeria do PowerShell. Depois que o provedor está instalado e importado, você pode instalar pacotes do Windows.

Execute os comandos a seguir na sessão do PowerShell que foi criada anteriormente:

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Para verificar rapidamente se o IIS está configurado corretamente, você pode visitar a URL `http://192.168.1.10/` e deverá ver uma página inicial. Quando o IIS é instalado, um site chamado `Default Web Site` escutando na porta 80 é criado por padrão.

## <a name="install-the-aspnet-core-module"></a>Instalar o módulo do ASP.NET Core

O módulo do ASP.NET Core é um módulo IIS 7.5+ responsável pelo gerenciamento de processos de ouvintes HTTP do ASP.NET Core e solicitações de proxy para processos que ele gerencia. No momento, o processo para instalar o módulo do ASP.NET Core para IIS é manual. Instalar o [Pacote de hospedagem do .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) em um computador normal (não nano). Depois de instalar o pacote em um computador normal, copie os arquivos a seguir para o compartilhamento de arquivos que criamos anteriormente.

Em um servidor normal (não Nano) com o IIS, execute os seguintes comandos de cópia:

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

Substitua `C:\windows\system32\inetsrv` com `C:\Program Files\IIS Express` em um computador com Windows 10.

No lado do Nano, você precisará copiar os seguintes arquivos do compartilhamento de arquivos que criamos anteriormente para os locais válidos. Portanto, execute os seguintes comandos de cópia:

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

Execute o seguinte script na sessão remota:

[!code-powershell[](nano-server/enable-aspnetcoremodule.ps1)]

> [!NOTE]
> Exclua os arquivos *aspnetcore.dll* e *aspnetcore_schema.xml* do compartilhamento após a etapa acima.

## <a name="installing-net-core-framework"></a>Instalando o .NET Core Framework

Se seu aplicativo é publicado como uma [FDD (implantação dependente de estrutura)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), o .NET Core deve ser instalado no servidor. Use o [script do PowerShell dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) em uma sessão remota do PowerShell para instalar o .NET Framework em seu Nano Server. Passe a versão do CLI com a opção `-Version`:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Publicando o aplicativo

Copie a saída publicada do seu aplicativo existente para a raiz do compartilhamento de arquivos.

Talvez seja necessário fazer alterações no seu *web.config* para apontar o local para o qual você extraiu *dotnet.exe*. Como alternativa, você pode adicionar *dotnet.exe* ao seu caminho.

Exemplo de como pode ser a aparência de um *web.config* se *dotnet.exe* **não** está no caminho:

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

Execute os comandos a seguir na sessão remota para criar um novo site no IIS para o aplicativo publicado em uma porta diferente da usada no site padrão. Você também precisa abrir aquela porta para acessar a Web. Esse script usa a `DefaultAppPool` para manter a simplicidade. Para obter mais considerações sobre a execução em um pool de aplicativos, consulte [Pools de aplicativos](xref:host-and-deploy/iis/index#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Problema conhecido executando a CLI do .NET Core no Nano Server e solução alternativa
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Executando o aplicativo

O aplicativo Web publicado é acessível em um navegador em `http://192.168.1.10:8000`. Se você tiver configurado o log conforme descrito em [Criação e redirecionamento de Log](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), você poderá exibir os logs em *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
