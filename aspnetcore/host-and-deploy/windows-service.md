---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f9b1c3fbfafa839c116688e0ac63804afcd5dbe0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206667"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedar o ASP.NET Core em um serviço Windows

Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando hospedado como um Serviço Windows, o aplicativo é iniciado automaticamente após a reinicialização.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([como baixar](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Converter um projeto em um serviço Windows

As alterações mínimas a seguir são necessárias para configurar um projeto existente do ASP.NET Core a ser executado como um serviço:

1. No arquivo de projeto:

   * Confirme a presença de um [RID (Identificador de Tempo de Execução)](/dotnet/core/rid-catalog) do Windows ou adicione-a ao `<PropertyGroup>` que contém a estrutura de destino:

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      Para publicar para vários RIDs:

      * Forneça os RIDs em uma lista delimitada por ponto e vírgula.
      * Use o nome da propriedade `<RuntimeIdentifiers>` (plural).

      Para obter mais informações, consulte [Catálogo de RID do .NET Core](/dotnet/core/rid-catalog).

   * Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).

1. Faça as seguintes alterações em `Program.Main`:

   * Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.

   * Chame [UseContentRoot](xref:fundamentals/host/web-host#content-root) e use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publique o aplicativo. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles). Ao usar o Visual Studio, selecione **FolderProfile**.

   Para publicar o aplicativo de exemplo usando as ferramentas da CLI (interface de linha de comando), execute o comando [dotnet publish](/dotnet/core/tools/dotnet-publish) em um prompt de comando da pasta do projeto. O RID deve ser especificado na propriedade `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) do arquivo de projeto. No exemplo a seguir, o aplicativo é publicado na Configuração de versão para o tempo de execução `win7-x64`:

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço. O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável. **O espaço entre o sinal de igual e o caractere de aspas no início do caminho é necessário.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Para um serviço publicado na pasta do projeto, use o caminho para a pasta *publish* para criar o serviço. No exemplo a seguir:

   * O projeto reside na pasta *c:\\my_services\\AspNetCoreService*.
   * O projeto é publicado na configuração `Release`.
   * O TFM (Moniker da Estrutura de Destino) é `netcoreapp2.1`.
   * O RID (Identificador de Tempo de Execução) é `win7-x64`.
   * O nome do executável de aplicativo é *AspNetCoreService.exe*.
   * O nome do serviço é **MyService**.

   Exemplo:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > Verifique se existe o espaço entre o argumento `binPath=` e seu valor.

   Para publicar e iniciar o serviço de uma pasta diferente:

      * Use a opção [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) no comando `dotnet publish`. Se você usar o Visual Studio, configure o **Local de Destino** na página de propriedades da publicação **FolderProfile** antes de selecionar o botão **Publicar**.
      * Crie o serviço com o comando `sc.exe` usando o caminho da pasta de saída. Inclua o nome do executável do serviço no caminho fornecido para `binPath`.

1. Inicie o serviço com o comando `sc start <SERVICE_NAME>`.

   Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:

   ```console
   sc start MyService
   ```

   O comando leva alguns segundos para iniciar o serviço.

1. Para verificar o status do serviço, use o comando `sc query <SERVICE_NAME>`. O status é relatado como um dos seguintes valores:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Use o seguinte comando para verificar o status do serviço de aplicativo de exemplo:

   ```console
   sc query MyService
   ```

1. Quando o serviço estiver no estado `RUNNING` e se o serviço for um aplicativo Web, procure o aplicativo em seu caminho (por padrão, `http://localhost:5000`, que redireciona para `https://localhost:5001` ao usar [Middleware de Redirecionamento HTTPS](xref:security/enforcing-ssl)).

   Para o serviço de aplicativo de exemplo, procure o aplicativo em `http://localhost:5000`.

1. Interrompa o serviço com o comando `sc stop <SERVICE_NAME>`.

   O comando a seguir interrompe o serviço de aplicativo de exemplo:

   ```console
   sc stop MyService
   ```

1. Após um pequeno atraso para interromper um serviço, desinstale o serviço com o comando `sc delete <SERVICE_NAME>`.

   Verifique o status do serviço de aplicativo de exemplo:

   ```console
   sc query MyService
   ```

   Quando o serviço de aplicativo de exemplo estiver no estado `STOPPED`, use o seguinte comando para desinstalar o serviço de aplicativo de exemplo:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Execute o aplicativo fora de um serviço

É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições. Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Manipular eventos de início e de parada

Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:

1. Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` não é passado do `Main` para o `CreateWebHostBuilder`, porque a assinatura do `CreateWebHostBuilder` deve ser `CreateWebHostBuilder(string[])` para que o [teste de integração](xref:test/integration-tests) funcione corretamente.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional. Para obter mais informações, consulte <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>Configurar o HTTPS

Especifique uma [configuração do ponto de extremidade HTTPS do servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="current-directory-and-content-root"></a>Diretório atual e a raiz do conteúdo

O diretório de trabalho atual retornado ao chamar `Directory.GetCurrentDirectory()` de um serviço Windows é a pasta *C:\\WINDOWS\\system32*. A pasta *system32* não é um local adequado para armazenar os arquivos de um serviço (por exemplo, os arquivos de configurações). Use uma das seguintes abordagens para manter e acessar os arquivos de ativos e de configurações de um serviço com [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) ao usar um [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):

* Use o caminho raiz do conteúdo. O `IHostingEnvironment.ContentRootPath` é o mesmo caminho fornecido para o argumento `binPath` quando o serviço é criado. Em vez de usar `Directory.GetCurrentDirectory()` para criar caminhos para arquivos de configurações, use o caminho raiz do conteúdo e mantenha os arquivos na raiz do conteúdo do aplicativo.
* Armazene os arquivos em um local adequado no disco. Especifique um caminho absoluto com `SetBasePath` para a pasta que contém os arquivos.

## <a name="additional-resources"></a>Recursos adicionais

* [Configuração do ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclui a configuração HTTPS e suporte à SNI)
* <xref:fundamentals/host/web-host>
