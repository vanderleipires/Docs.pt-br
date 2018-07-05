---
title: Hospedar o ASP.NET Core em um serviço Windows
author: guardrex
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 718cc83bb29c0cff323853d22c107e00616b1dd1
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126229"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedar o ASP.NET Core em um serviço Windows

Por [Luke Latham](https://github.com/guardrex) e [Tom Dykstra](https://github.com/tdykstra)

Um aplicativo ASP.NET Core pode ser hospedado no Windows sem usar o IIS como um [Serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando hospedado como um serviço Windows, o aplicativo pode iniciar automaticamente após reinicializações e falhas, sem exigir intervenção humana.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Introdução

As seguintes alterações mínimas são necessárias para configurar um projeto existente do ASP.NET Core para ser executado em um serviço:

1. No arquivo de projeto:

   1. Confirme a presença do identificador de tempo de execução ou adicione-o ao **\<PropertyGroup>** que contém a estrutura de destino:
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. Adicionar uma referência de pacote para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Faça as seguintes alterações em `Program.Main`:

   * Chame [host. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice), em vez de `host.Run`.

   * Se o código chama `UseContentRoot`, use um caminho para o local publicado do aplicativo, em vez de `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publique o aplicativo. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).

   Para publicar o aplicativo de exemplo da linha de comando, execute o seguinte comando em uma janela do console da pasta de projeto:

   ```console
   dotnet publish --configuration Release
   ```

1. Use a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar o serviço. O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável. **O espaço entre o sinal de igual e o caractere de aspas no início do caminho é necessário.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Para um serviço publicado na pasta do projeto, use o caminho para a pasta *publish* para criar o serviço. No exemplo a seguir, o serviço é:

   * Chamado **MyService**.
   * Publicado na pasta *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish*.
   * Representado por um executável de aplicativo chamado *AspNetCoreService.exe*.

   Abra um shell de comando com privilégios administrativos e execute o seguinte comando:

   ```console
   sc create MyService binPath= "c:\my_services\aspnetcoreservice\bin\release\<TARGET_FRAMEWORK>\publish\aspnetcoreservice.exe"
   ```
   
   > [!IMPORTANT]
   > Verifique se existe o espaço entre o argumento `binPath=` e seu valor.
   
   Para publicar e iniciar o serviço de uma pasta diferente:
   
   1. Use a opção [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) no comando `dotnet publish`.
   1. Crie o serviço com o comando `sc.exe` usando o caminho da pasta de saída. Inclua o nome do executável do serviço no caminho fornecido para `binPath`.

1. Inicie o serviço com o comando `sc start <SERVICE_NAME>`.

   Para iniciar o serviço de aplicativo de exemplo, use o seguinte comando:

   ```console
   sc start MyService
   ```

   O comando leva alguns segundos para iniciar o serviço.

1. O comando `sc query <SERVICE_NAME>` pode ser usado para verificar o status do serviço para determinar seu status:

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

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornecer uma maneira de executar fora de um serviço

É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições. Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

Porque a configuração do ASP.NET Core requer pares nome-valor para argumentos de linha de comando, a opção `--console` é removida antes que os argumentos sejam passados para [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Manipular eventos de início e de parada

Para tratar eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) e [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), faça as seguintes alterações adicionais:

1. Crie uma classe que derive de [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Crie um método de extensão para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que passe o `WebHostService` personalizado para [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Se o código `WebHostService` personalizado exigir um serviço de injeção de dependência (como um agente), obtenha-o da propriedade [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="kestrel-endpoint-configuration"></a>Configuração de ponto de extremidade do Kestrel

Para obter informações sobre a configuração de ponto de extremidade Kestrel, incluindo configuração do HTTPS e suporte a SNI, veja [configuração de ponto de extremidade Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).
