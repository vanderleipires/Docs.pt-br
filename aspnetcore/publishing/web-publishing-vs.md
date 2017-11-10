---
title: "Criar perfis de publicação para o Visual Studio e o MSBuild"
author: rick-anderson
description: "Explica a publicação na Web no Visual Studio."
keywords: "ASP.NET Core, publicação na Web, publicação, msbuild, implantação da Web, dotnet publicar, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Criar perfis de publicação para o Visual Studio e o MSBuild para implantar aplicativos ASP.NET Core

Por [Sayed Hashimi de Ibrahim](https://github.com/sayedihashimi) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Este artigo se concentra no uso do Visual Studio 2017 para criar perfis de publicação. Os perfis de publicação criados com o Visual Studio podem ser executados do MSBuild e do Visual Studio 2017. O artigo fornece detalhes do processo de publicação. Consulte [Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obter instruções sobre a publicação no Azure.

O arquivo *.csproj* a seguir foi criado com o comando `dotnet new mvc`:

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

O atributo `Sdk` no elemento `<Project>` (na primeira linha) da marcação acima faz o seguinte:

* Importa o arquivo `props` *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* no início.
* Importa o arquivo de destino de *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* no fim.

O local padrão para `MSBuildSDKsPath` (com o Visual Studio Enterprise 2017) é a pasta *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` depende de:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Que faz com que os seguintes `props` e `targets` sejam importados:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Destinos de publicação importarão novamente o conjunto certo de destinos com base no método de publicação usado.

Quando o MSBuild ou o Visual Studio carrega um projeto, as seguintes ações de nível alto são executadas:

* Compilar projeto
* Computar arquivos a publicar
* Publicar arquivos para o destino

### <a name="compute-project-items"></a>Itens de projeto de computação

Quando o projeto é carregado, os itens de projeto (arquivos) são computados. O atributo `item type` determina como o arquivo é processado. Por padrão, os arquivos *.cs* são incluídos na lista de itens `Compile`. Os arquivos na lista de itens `Compile` são compilados.

A lista de itens `Content` contém arquivos que serão publicados juntamente com as saídas de build. Por padrão, os arquivos correspondendo ao padrão wwwroot/** serão incluídos no item `Content`. [wwwroot/** é um padrão de recurso de curinga](https://gruntjs.com/configuring-tasks#globbing-patterns) que especifica todos os arquivos na pasta *wwwroot* **e** subpastas. Para adicionar explicitamente um arquivo à lista de publicação, adicione o arquivo diretamente no arquivo *.csproj* conforme mostrado em [Incluindo arquivos](#including-files).

Quando você seleciona o botão **Publicar** no Visual Studio ou quando você publica da linha de comando:

- Os itens/propriedades são calculados (os arquivos necessários para compilar).
- Somente Visual Studio: pacotes NuGet são restaurados.  (A restauração precisa ser explícita pelo usuário na CLI.)
- O projeto é compilado.
- Os itens de publicação são computados (os arquivos necessários para a publicação).
- O projeto é publicado. (Os arquivos computados são copiados para o destino de publicação.)

## <a name="basic-command-line-publishing"></a>Publicação de linha de comando básica

Esta seção funciona em todas as plataformas com suporte do .NET Core e não requer o Visual Studio. Nas amostras abaixo, o comando `dotnet publish` é executado no diretório do projeto (que contém o arquivo *.csproj*). Se você não estiver na pasta do projeto, você poderá passar explicitamente no caminho do arquivo de projeto. Por exemplo:

```console
dotnet publish  c:/webs/web1
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

--------------

O `dotnet publish` produz uma saída semelhante à seguinte:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

A pasta de publicação padrão é `bin\$(Configuration)\netcoreapp<version>\publish`. O padrão de `$(Configuration)` é Depurar. Na amostra acima, o `<TargetFramework>` é `netcoreapp2.0`.

`dotnet publish -h` exibe informações de ajuda para publicação.

O comando a seguir especifica um build de `Release` e o diretório de publicação:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

O comando `dotnet publish` chama `MSBuild`, que invoca o destino `Publish`. Quaisquer parâmetros passados para `dotnet publish` são passados para `MSBuild`. O parâmetro `-c` é mapeado para a propriedade do MSBuild `Configuration`. O parâmetro `-o` é mapeado para `OutputPath`.

Você pode passar propriedades do MSBuild usando qualquer um dos seguintes formatos:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

O comando a seguir publica um build de `Release` para um compartilhamento de rede:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

O compartilhamento de rede é especificado com barras "/" (*//r8/*) e funciona em todas as plataformas com suporte do .NET Core.

Confirme que o aplicativo publicado para implantação não está em execução. Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução. A implantação não pode ocorrer porque arquivos bloqueados não podem ser copiados.

## <a name="publish-profiles"></a>Perfis de publicação

Esta seção usa o Visual Studio 2017 e superior para criar perfis de publicação. Uma vez criados, você pode publicar do Visual Studio ou da linha de comando.

Perfis de publicação podem simplificar o processo de publicação. Você pode ter vários perfis de publicação. Para criar um perfil de publicação no Visual Studio, clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar**. Como alternativa, você pode selecionar **Publicar     \<nome do projeto>** no menu do build. A guia **Publicar** da página de capacidades do aplicativo é exibida. Se o projeto não contiver um perfil de publicação, a página a seguir será exibida:

![A guia **Publicar** da página de capacidades de aplicativo mostrando Azure, IIS, FTB e Pasta, com a opção Azure selecionada. Ela também mostra os botões de opção Criar Novo e Selecionar Existente](web-publishing-vs/_static/az.png)

Quando a opção **Pasta** é selecionada, o botão **Publicar** cria um perfil de publicação de pasta e publica.

![A guia **Publicar** da página de capacidades de aplicativo mostrando Azure, IIS, FTB e Pasta](web-publishing-vs/_static/pub1.png)

Depois que um perfil de publicação é criado, a guia **Publicar** é alterada e você seleciona **Criar novo perfil** para criar um novo perfil.

![A guia **Publicar** da página de recursos de aplicativo mostrando FolderProfile -created na última etapa o link Criar novo perfil](web-publishing-vs/_static/create_new.png)

O Assistente de publicação dá suporte aos destinos de publicação a seguir:

- Serviço de Aplicativo do Microsoft Azure
- IIS, FTP, etc. (para qualquer servidor Web)
- Pasta
- Importar perfil (permite que você importe um perfil).
- Máquinas Virtuais do Microsoft Azure

Para obter mais informações, consulte [Quais opções de publicação são adequadas para mim?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).

Quando você cria um perfil de publicação com o Visual Studio, um arquivo do MSBuild *Properties/PublishProfiles/\<nome da publicação>.pubxml* é criado. Esse arquivo *.pubxml* é um arquivo do MSBuild e contém definições de configuração de publicação. Você pode alterar esse arquivo para personalizar o build e o processo de publicação. Esse arquivo é lido pelo processo de publicação. `<LastUsedBuildConfiguration>` é especial porque é uma propriedade global e não deverá estar em nenhum arquivo que será importado no build. Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).
O check-in do arquivo *.pubxml* não deve ser feito no controle do código-fonte porque ele depende do arquivo *.user*. O check-in do arquivo *.user* nunca deve ser feito no controle do código-fonte porque ele pode conter informações confidenciais e só é válido para um usuário e computador.

Informações confidenciais (como a senha de publicação) são criptografadas em um nível por usuário/computador e armazenadas no arquivo *Properties/PublishProfiles/\<nome da publicação>.pubxml.user*. Como esse arquivo pode conter informações confidenciais, o check-in dele **não** deve ser realizado no controle do código-fonte.

Para obter uma visão geral de como publicar um aplicativo Web do ASP.NET Core, consulte [Publicação e implantação](index.md). [Publicação e implantação](index.md) é um projeto de software livre em https://github.com/aspnet/websdk.

 O `dotnet publish` pode usar os perfis de publicação de uma pasta, Msdeploy e [KUDU](https://github.com/projectkudu/kudu/wiki):
 
Pasta (funciona em diferentes plataformas) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

Msdeploy (atualmente só funciona no Windows, uma vez que o Msdeploy não funciona em diferentes plataformas): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Pacote do Msdeploy (atualmente só funciona no Windows, uma vez que o Msdeploy não funciona em diferentes plataformas): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

Nos exemplos anteriores, **não** passe o `deployonbuild` para `dotnet publish`.

Para obter mais informações, confira [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)

O `dotnet publish` oferece suporte a APIs do KUDU para publicar no Azure de qualquer plataforma. A publicação do Visual Studio suporta APIs do KUDU, mas ela é suportada pelo websdk para publicação em diferentes plataformas para o Azure.

Adicionar um perfil de publicação à pasta *Propriedades/PerfisDePublicação* com o seguinte conteúdo:

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

Executar o seguinte comando irá compactar os conteúdos de publicação e publicá-los no Azure usando as APIs do KUDU.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Defina as seguintes propriedades de MSBuild ao usar um perfil de publicação:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Por exemplo, ao publicar com um perfil chamado *FolderProfile*, você pode executar qualquer um dos comandos abaixo.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Quando você invoca `dotnet build`, ele chama `msbuild` para executar o build e o processo de publicação. Quando você passa um perfil de pasta, chamar `dotnet build` ou `msbuild` é essencialmente equivalente. Ao chamar o MSBuild diretamente no Windows, você obterá a versão do .NET Framework do MSBuild.  O MSDeploy é atualmente limitado a computadores Windows para a publicação. Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild que, por sua vez, usa MSDeploy em perfis não de pasta. Chamar `dotnet build` em um perfil não de pasta invoca o MSBuild (usando MSDeploy) e resulta em uma falha (mesmo quando em execução em uma plataforma do Windows). Para publicar com um perfil não de pasta, chame o MSBuild diretamente.

A pasta de perfil de publicação a seguir foi criada com o Visual Studio e publica em um compartilhamento de rede:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Observe que `<LastUsedBuildConfiguration>` é definido como `Release`.  Ao publicar no Visual Studio, o valor da propriedade de configuração `<LastUsedBuildConfiguration>` é definido usando o valor quando o processo de publicação é iniciado. A propriedade de configuração `<LastUsedBuildConfiguration>` é especial e não deve ser substituída em um arquivo do MSBuild importado. Você pode substituir essa propriedade da linha de comando. Por exemplo:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Usando o MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publicar em um ponto de extremidade do MSDeploy da linha de comando

Como mencionado anteriormente, você pode publicar usando o comando `dotnet publish` ou `msbuild`. `dotnet publish` é executado no contexto do .NET Core. `msbuild` requer o .NET framework e, portanto, é limitado a ambientes Windows.

A maneira mais fácil publicar com MSDeploy é primeiro criar um perfil de publicação no Visual Studio de 2017 e usar o perfil da linha de comando.

Na amostra a seguir, um aplicativo Web do ASP.NET Core é criado (usando `dotnet new mvc`) e um perfil de publicação do Azure é adicionado com o Visual Studio.

Você executa `msbuild` de um **Prompt de Comando do Desenvolvedor para VS 2017**. O Prompt de comando do desenvolvedor terá o *msbuild.exe* correto em seu caminho e definirá algumas variáveis do MSBuild.

O MSBuild usa a sintaxe a seguir:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Você pode obter o `Password` do arquivo *\<Nome da publicação>.PublishSettings*. Você pode baixar o arquivo *.PublishSettings*:

- Gerenciador de Soluções: clique com o botão direito do mouse no aplicativo Web e selecione **Baixar Perfil de Publicação**.
- O Portal de Gerenciamento do Azure: selecione **Obter perfil de publicação** na folha Aplicativo Web.

`Username` pode ser encontrado no perfil de publicação.

A amostra a seguir usa o perfil de publicação "Web11112 – implantação da Web":

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Excluindo arquivos

Ao publicar aplicativos Web do ASP.NET Core, os artefatos de build e o conteúdo da pasta *wwwroot* são incluídos. `msbuild` dá suporte a [padrões de caractere curinga](https://gruntjs.com/configuring-tasks#globbing-patterns). Por exemplo, a marcação de elemento `<Content>` a seguir excluirá todos os arquivos de texto (*.txt*) da pasta *wwwroot/content* e de todas as suas subpastas.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

A marcação acima pode ser adicionada a um perfil de publicação ou ao arquivo *.csproj*. Quando adicionada ao arquivo *.csproj*, a regra será adicionada a todos os perfis de publicação no projeto.

A marcação do elemento `<MsDeploySkipRules>` a seguir exclui todos os arquivos da pasta *wwwroot/content*:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` não excluirá os destinos de *omissão* do site de implantação. Pastas e arquivos de destino de `<Content>` serão excluídos do site de implantação. Por exemplo, suponha que você implantou um aplicativo Web com os seguintes arquivos:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Se você tivesse adicionado a marcação `<MsDeploySkipRules>` a seguir, esses arquivos não seriam excluídos no site de implantação.

``` xml
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

A marcação `<MsDeploySkipRules>` mostrada acima impede que os arquivos *ignorados* sejam implantados, mas não exclui os arquivos depois de eles serem implantados.

A marcação `<Content>` a seguir excluiria os arquivos de destino no local de implantação:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Usar a implantação de linha de comando com a marcação `<Content>` acima resultaria em uma saída semelhante à seguinte:

``` console
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

## <a name="including-files"></a>Incluindo arquivos

A marcação a seguir pode ser usada para incluir uma pasta *images* fora do diretório do projeto para a pasta *wwwroot/images* do site de publicação.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

A marcação pode ser adicionada ao arquivo *.csproj* ou ao perfil de publicação. Se ela for adicionada ao arquivo *.csproj*, ela será incluída em cada perfil de publicação no projeto.

A marcação realçada a seguir mostra como:

* Copiar um arquivo de fora do projeto para a pasta *wwwroot*.
* Excluir a pasta *wwwroot\Content*.
* Excluir *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Consulte o [Leiame do WebSDK](https://github.com/aspnet/websdk) para obter mais amostras de implantação.

### <a name="run-a-target-before-or-after-publishing"></a>Executar um destino antes ou depois da publicação

Os destinos `BeforePublish` e `AfterPublish` internos podem ser usados para executar um destino antes ou após o destino de publicação. A marcação a seguir pode ser adicionada ao perfil de publicação para registrar mensagens de saída do console antes e após a publicação:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>O serviço Kudu

Para exibir os arquivos em seu aplicativo Web do Azure, use o [serviço kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Acrescente o token `scm` ao nome ou ao seu aplicativo Web. Por exemplo:

| URL               | Resultado|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Aplicativo Web |
| `http://mysite.scm.azurewebsites.net/` | Serviço kudu |

Selecione o item de menu [Console de Depuração](https://github.com/projectkudu/kudu/wiki/Kudu-console) para exibir/editar/excluir/adicionar arquivos.

## <a name="additional-resources"></a>Recursos adicionais

- A [Implantação da Web](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) simplifica a implantação de aplicativos Web e sites da Web em servidores IIS.

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): problemas de arquivos e recursos de solicitação para implantação.
