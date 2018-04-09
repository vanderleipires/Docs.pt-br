---
title: Host ASP.NET Core em um serviço do Windows
author: tdykstra
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço do Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host ASP.NET Core em um serviço do Windows

Por [Tom Dykstra](https://github.com/tdykstra)

A maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS é executá-lo uma [serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando hospedado como um serviço do Windows, o aplicativo pode automaticamente iniciar após reinicializar e falhas sem exigir intervenção humana.

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)). Para obter instruções sobre como executar o aplicativo de exemplo, consulte o exemplo *README.md* arquivo.

## <a name="prerequisites"></a>Pré-requisitos

* O aplicativo deve ser executado em tempo de execução do .NET Framework. No *. csproj* de arquivos, especifique os valores apropriados para [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Veja um exemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Ao criar um projeto no Visual Studio, use o **aplicativo do ASP.NET Core (.NET Framework)** modelo.

* Se o aplicativo recebe solicitações de Internet (não apenas a partir de uma rede interna), ele deve usar o [HTTP.sys](xref:fundamentals/servers/httpsys) servidor web (anteriormente conhecida como [WebListener](xref:fundamentals/servers/weblistener) para aplicativos do ASP.NET Core 1. x) em vez de [Kestrel](xref:fundamentals/servers/kestrel). O IIS é recomendado para uso como um servidor proxy reverso com Kestrel para implantações de borda. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Introdução

Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.

1. Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Faça as seguintes alterações em `Program.Main`:

   * Chamar `host.RunAsService` em vez de `host.Run`.

   * Se o código chama `UseContentRoot`, use um caminho para o local de publicação em vez de `Directory.GetCurrentDirectory()`.

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. Publica o aplicativo em uma pasta. Use [dotnet publicar](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica uma pasta.

4. Teste ao criar e iniciar o serviço.

   Abra um shell de comando com privilégios administrativos para usar o [sc.exe](https://technet.microsoft.com/library/bb490995) ferramenta de linha de comando para criar e iniciar um serviço. Se o serviço é chamado MyService, publicados `c:\svc`, e denominado AspNetCoreService, os comandos são:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   O `binPath` valor é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.

   ![Janela de console criar e iniciar o exemplo](windows-service/_static/create-start.png)

   Quando terminar desses comandos, navegue até o mesmo caminho que durante a execução como um aplicativo de console (por padrão, `http://localhost:5000`):

   ![Executando em um serviço](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornecem uma maneira de executar fora de um serviço

É mais fácil de testar e depurar quando em execução fora de um serviço, portanto, é comum para adicionar o código que chama `RunAsService` apenas em determinadas condições. Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um `--console` argumento de linha de comando ou se o depurador é anexado:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a>Tratar parar e iniciar eventos

Para tratar `OnStarting`, `OnStarted`, e `OnStopping` eventos, faça as seguintes alterações adicionais:

1. Criar uma classe que deriva de `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Criar um método de extensão para `IWebHost` que passa personalizado `WebHostService` para `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de `RunAsService`:

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
Se personalizado `WebHostService` código requer que um serviço de injeção de dependência (como um agente de log), ele obtido o `Services` propriedade `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de Balanceador de carga

Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estiverem atrás de um proxy ou balanceador de carga podem exigir configuração adicional. Para obter mais informações, consulte [configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Agradecimentos

Este artigo foi escrito com a Ajuda das fontes publicados:

* [Hospedagem ASP.NET Core como serviço do Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Como hospedar o ASP.NET Core em um serviço do Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
