---
title: Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio
author: rick-anderson
description: Saiba como criar perfis de publicação no Visual Studio e usá-los para gerenciar implantações de aplicativo do ASP.NET Core para vários destinos.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Perfis para a implantação de aplicativo do ASP.NET Core de publicação do Visual Studio

Por [Sayed Hashimi de Ibrahim](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se concentra no uso de 2017 do Visual Studio para criar e usar perfis de publicação. Os perfis de publicação criados com o Visual Studio podem ser executados do MSBuild e do Visual Studio 2017. Consulte [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obter instruções sobre a publicação no Azure.

O seguinte arquivo de projeto foi criado com o comando `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

O `<Project>` do elemento `Sdk` atributo realiza as seguintes tarefas:

* Importa o arquivo de propriedades de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* no início.
* Importa o arquivo de destino de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* no fim.

O local padrão para `MSBuildSDKsPath` (com o Visual Studio Enterprise 2017) é a pasta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

O `Microsoft.NET.Sdk.Web` depende do SDK:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Que faz com que as propriedades e os destinos a serem importados a seguir:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Publica o direito de conjunto de destinos com base no método de publicação usado de importação de destinos.

Quando o MSBuild ou o Visual Studio carrega um projeto, ocorrem as seguintes ações de alto nível:

* Compilar projeto
* Computar arquivos a publicar
* Publicar arquivos para o destino

## <a name="compute-project-items"></a>Itens de projeto de computação

Quando o projeto é carregado, os itens de projeto (arquivos) são computados. O atributo `item type` determina como o arquivo é processado. Por padrão, os arquivos *.cs* são incluídos na lista de itens `Compile`. Os arquivos na lista de itens `Compile` são compilados.

O `Content` à lista de itens contém arquivos que são publicados além das saídas de compilação. Por padrão, arquivos correspondentes ao padrão `wwwroot/**` são incluídos no `Content` item. O `wwwroot/\*\*` [padrão de globalização](https://gruntjs.com/configuring-tasks#globbing-patterns) corresponde a todos os arquivos de *wwwroot* pasta **e** subpastas. Para adicionar explicitamente um arquivo à lista de publicação, adicione o arquivo diretamente no *. csproj* arquivo conforme mostrado na [arquivos de inclusão](#include-files).

Ao selecionar o **publicar** botão no Visual Studio ou a publicação da linha de comando:

* Os itens/propriedades são calculados (os arquivos necessários para compilar).
* **O Visual Studio só**: pacotes do NuGet serão restaurados. (A restauração precisa ser explícita pelo usuário na CLI.)
* O projeto é compilado.
* Os itens de publicação são computados (os arquivos necessários para a publicação).
* O projeto é publicado (computados arquivos são copiados para o destino de publicação).

Quando faz referência a um projeto do ASP.NET Core `Microsoft.NET.Sdk.Web` no arquivo de projeto, um *app_offline.htm* arquivo é colocado na raiz do diretório de aplicativo da web. Quando o arquivo estiver presente, o módulo do ASP.NET Core apenas desligará o aplicativo e servirá o arquivo *app_offline.htm* durante a implantação. Para obter mais informações, consulte [Referência de configuração do módulo do ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Publicação de linha de comando básica

Publicação de linha de comando funciona em todas as plataformas com suporte ao .NET Core e não requer o Visual Studio. No exemplo abaixo, o [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando é executado no diretório do projeto (que contém o *. csproj* arquivo). Se não estiver na pasta do projeto, passar explicitamente no caminho do arquivo de projeto. Por exemplo:

```console
dotnet publish C:\Webs\Web1
```

Execute os comandos a seguir para criar e publicar um aplicativo Web:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

O [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando produz saída semelhante à seguinte:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

A pasta de publicação padrão é `bin\$(Configuration)\netcoreapp<version>\publish`. O padrão para `$(Configuration)` é *depurar*. No exemplo anterior, o `<TargetFramework>` é `netcoreapp2.0`.

`dotnet publish -h` exibe informações de ajuda para publicação.

O comando a seguir especifica um build de `Release` e o diretório de publicação:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

O [dotnet publicar](/dotnet/core/tools/dotnet-publish) comando chamadas MSBuild, que chama o `Publish` destino. Todos os parâmetros passados para `dotnet publish` são passados para o MSBuild. O parâmetro `-c` é mapeado para a propriedade do MSBuild `Configuration`. O parâmetro `-o` é mapeado para `OutputPath`.

Propriedades MSBuild podem ser passadas usando qualquer um dos seguintes formatos:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

O comando a seguir publica um build de `Release` para um compartilhamento de rede:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

O compartilhamento de rede é especificado com barras "/" (*//r8/*) e funciona em todas as plataformas com suporte do .NET Core.

Confirme que o aplicativo publicado para implantação não está em execução. Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução. A implantação não pode ocorrer porque arquivos bloqueados não podem ser copiados.

## <a name="publish-profiles"></a>Perfis de publicação

Esta seção usa 2017 do Visual Studio para criar um perfil de publicação. Depois de criado, a publicação do Visual Studio ou a linha de comando está disponível.

Publicar perfis podem simplificar o processo de publicação, e podem existir vários perfis. Crie um perfil de publicação no Visual Studio escolhendo um dos seguintes caminhos:

* Clique com botão direito no projeto no Gerenciador de soluções e selecione **publicar**.
* Selecione **publicar &lt;project_name&gt;**  do **criar** menu.

O **publicar** guia da página de recursos do aplicativo é exibida. Se o projeto não tiver um perfil de publicação, a página seguinte é exibida:

![Guia Publicar da página de recursos do aplicativo](visual-studio-publish-profiles/_static/az.png)

Quando **pasta** é selecionada, especifique um caminho de pasta para armazenar os recursos publicados. A pasta padrão é *bin\Release\PublishOutput*. Clique o **criar perfil** botão para concluir.

Depois de criar um perfil de publicação, o **publicar** guia alterações. O perfil criado recentemente é exibida em uma lista suspensa. Clique em **criar novo perfil** para criar outro novo perfil.

![Guia Publicar da página de recursos de aplicativo mostrando FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

O Assistente de publicação dá suporte aos destinos de publicação a seguir:

* Serviço de Aplicativo do Azure
* Máquinas Virtuais do Azure
* IIS, FTP, etc. (para qualquer servidor web)
* Pasta
* Importar perfil

Para obter mais informações, consulte [quais opções de publicação são certos para mim](/visualstudio/ide/not-in-toc/web-publish-options).

Ao criar um perfil de publicação com o Visual Studio, um *propriedades/PublishProfiles/&lt;profile_name&gt;. pubxml* arquivo MSBuild é criado. O *. pubxml* arquivo é um arquivo MSBuild e contém definições de configuração de publicação. Esse arquivo pode ser alterado para personalizar a compilação e o processo de publicação. Esse arquivo é lido pelo processo de publicação. `<LastUsedBuildConfiguration>` é especial porque é uma propriedade global e não deve estar em qualquer arquivo que será importado no build. Consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) para obter mais informações.

Ao publicar em um destino do Azure, o *. pubxml* arquivo contém o identificador de assinatura do Azure. Com esse tipo de destino, adicionar esse arquivo ao controle de origem não é recomendado. Ao publicar em um destino não Azure, é seguro fazer check-in a *. pubxml* arquivo.

Informações confidenciais (como a senha de publicação) são criptografadas em um nível de usuário/computador. Ele é armazenado na *propriedades/PublishProfiles/&lt;profile_name&gt;. pubxml.user* arquivo. Como esse arquivo pode armazenar informações confidenciais, não deve ser verificado no controle de origem.

Para obter uma visão geral de como publicar um aplicativo web do ASP.NET Core, consulte [Host e implantar](xref:host-and-deploy/index). As tarefas de MSBuild e os destinos necessários para publicar um aplicativo do ASP.NET Core são o código-fonte aberto no https://github.com/aspnet/websdk.

`dotnet publish` pode usar a pasta, MSDeploy, e [Kudu](https://github.com/projectkudu/kudu/wiki) perfis de publicação:

Pasta (funciona entre plataformas):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (atualmente isso só funciona no Windows como MSDeploy não plataforma cruzada):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Pacote MSDeploy (atualmente isso só funciona no Windows como MSDeploy não plataforma cruzada):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

Nos exemplos anteriores, **não** passar `deployonbuild` para `dotnet publish`.

Para obter mais informações, consulte [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` oferece suporte a APIs do Kudu para publicar no Azure de qualquer plataforma. O Visual Studio publique dá suporte a APIs do Kudu, mas ele é suportado por WebSDK para várias plataformas publicar no Azure.

Adicionar um perfil de publicação para o *propriedades/PublishProfiles* pasta com o seguinte conteúdo:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Execute o seguinte comando para compactar o conteúdo de publicar e publicá-lo no Azure usando as APIs de Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Defina as seguintes propriedades de MSBuild ao usar um perfil de publicação:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Durante a publicação com um perfil chamado *FolderProfile*, qualquer um dos comandos a seguir podem ser executado:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Ao invocar [dotnet compilação](/dotnet/core/tools/dotnet-build), ele chama `msbuild` para executar a compilação e o processo de publicação. Chamar `dotnet build` ou `msbuild` é equivalente ao passar em um perfil de pasta. Ao chamar o MSBuild diretamente no Windows, a versão do .NET Framework do MSBuild é usada. O MSDeploy é atualmente limitado a computadores Windows para a publicação. Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild que, por sua vez, usa MSDeploy em perfis não de pasta. Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild (usando MSDeploy) e resulta em uma falha (mesmo quando em execução em uma plataforma do Windows). Para publicar com um perfil não de pasta, chame o MSBuild diretamente.

A pasta de perfil de publicação a seguir foi criada com o Visual Studio e publica em um compartilhamento de rede:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Observe que `<LastUsedBuildConfiguration>` é definido como `Release`. Ao publicar no Visual Studio, o valor da propriedade de configuração `<LastUsedBuildConfiguration>` é definido usando o valor quando o processo de publicação é iniciado. O `<LastUsedBuildConfiguration>` propriedade de configuração é especial e não deve ser substituída em um arquivo importado do MSBuild. Essa propriedade pode ser substituída na linha de comando.

Usando o .NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Usando o MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publicar em um ponto de extremidade do MSDeploy da linha de comando

A publicação pode ser feita usando o .NET Core CLI ou o MSBuild. `dotnet publish` é executado no contexto do .NET Core. O `msbuild` comando requer o .NET Framework, que limita a ambientes de Windows.

A maneira mais fácil publicar com MSDeploy é primeiro criar um perfil de publicação no Visual Studio de 2017 e usar o perfil da linha de comando.

No exemplo a seguir, é criado um aplicativo web do ASP.NET Core (usando `dotnet new mvc`), e um perfil de publicação do Azure é adicionado com o Visual Studio.

Executar `msbuild` de um **Prompt de comando do desenvolvedor para VS 2017**. O Prompt de comando do desenvolvedor tem corretas *msbuild.exe* em seu caminho com um conjunto de variáveis do MSBuild.

O MSBuild usa a sintaxe a seguir:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Obter o `Password` do  *\<nome da publicação >. PublishSettings* arquivo. Baixe o *. PublishSettings* arquivo do:

* Gerenciador de soluções: Clique no aplicativo Web e selecione **baixar perfil de publicação**.
* Portal do Azure: clique em **obter perfil de publicação** em seu aplicativo Web **visão geral** painel.

`Username` pode ser encontrado no perfil de publicação.

O exemplo a seguir usa o *Web11112 - implantação da Web* perfil de publicação:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Excluir arquivos

Ao publicar aplicativos Web do ASP.NET Core, os artefatos de build e o conteúdo da pasta *wwwroot* são incluídos. `msbuild` dá suporte a [padrões de caractere curinga](https://gruntjs.com/configuring-tasks#globbing-patterns). Por exemplo, a seguinte `<Content>` elemento exclui todo o texto (*. txt*) de arquivos do *wwwroot/content* pasta e todas as suas subpastas.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

A marcação anterior pode ser adicionada a um perfil de publicação ou o *. csproj* arquivo. Quando adicionada ao arquivo *.csproj*, a regra será adicionada a todos os perfis de publicação no projeto.

O seguinte `<MsDeploySkipRules>` elemento exclui todos os arquivos da *wwwroot/content* pasta:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Não exclua o *ignorar* destinos do site de implantação. `<Content>` pastas e arquivos de destino são excluídas do site de implantação. Por exemplo, suponha que um aplicativo da web implantados tinha os seguintes arquivos:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Se o seguinte `<MsDeploySkipRules>` elementos são adicionados, esses arquivos não serão excluídos no site de implantação.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Anterior `<MsDeploySkipRules>` elementos impedir o *ignorada* arquivos que está sendo implantado. Ele não excluirá os arquivos depois que são implantados.

O seguinte `<Content>` elemento exclui os arquivos de destino no local de implantação:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Com a implantação de linha de comando anterior `<Content>` elemento produz o seguinte resultado:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Incluir arquivos

A seguinte marcação inclui um *imagens* pasta fora do diretório do projeto para o *wwwroot/imagens* pasta do site de publicação:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

A marcação pode ser adicionada ao arquivo *.csproj* ou ao perfil de publicação. Se ela for adicionada para o *. csproj* arquivo, ele está incluído em cada perfil de publicação no projeto.

A marcação realçada a seguir mostra como:

* Copiar um arquivo de fora do projeto para a pasta *wwwroot*.
* Excluir a pasta *wwwroot\Content*.
* Excluir *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Consulte o [Leiame do WebSDK](https://github.com/aspnet/websdk) para obter mais amostras de implantação.

## <a name="run-a-target-before-or-after-publishing"></a>Executar um destino antes ou depois da publicação

O interno `BeforePublish` e `AfterPublish` destinos executam um destino antes ou após o destino de publicação. Adicione os seguintes elementos para o perfil de publicação para registrar em log mensagens de console antes e após a publicação:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publicar em um servidor usando um certificado não confiável

Adicionar o `<AllowUntrustedCertificate>` propriedade com um valor de `True` para o perfil de publicação:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>O serviço Kudu

Para exibir os arquivos em uma implantação de aplicativo web do serviço de aplicativo do Azure, use o [service Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Acrescente a `scm` token para o nome do aplicativo web. Por exemplo:

| URL                                    | Resultado       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Aplicativo Web      |
| `http://mysite.scm.azurewebsites.net/` | Serviço kudu |

Selecione o [Console depuração](https://github.com/projectkudu/kudu/wiki/Kudu-console) item de menu para exibir, editar, excluir ou adicionar arquivos.

## <a name="additional-resources"></a>Recursos adicionais

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifica a implantação de aplicativos web e sites para servidores IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Problemas de arquivos e recursos para implantação de solicitação.
