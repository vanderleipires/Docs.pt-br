---
title: Hospedar o ASP.NET Core em um serviço Windows
author: rick-anderson
description: Saiba como hospedar um aplicativo ASP.NET Core em um serviço Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153523"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hospedar o ASP.NET Core em um serviço Windows

Por [Tom Dykstra](https://github.com/tdykstra)

A maneira recomendada de hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS é executá-lo em um [serviço Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Quando hospedado como um serviço Windows, o aplicativo pode iniciar automaticamente após reinicializações e falhas, sem exigir intervenção humana.

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)). Para obter instruções sobre como executar o aplicativo de exemplo, confira o arquivo *README.md* do exemplo.

## <a name="prerequisites"></a>Pré-requisitos

* O aplicativo deve ser executado no tempo de execução do .NET Framework. No arquivo *.csproj*, especifique os valores apropriados para [TargetFramework](/nuget/schema/target-frameworks) e [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Veja um exemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Ao criar um projeto no Visual Studio, use o modelo **Aplicativo ASP.NET Core (.NET Framework)**.

* Se o aplicativo recebe solicitações da Internet (não apenas de uma rede interna), ele deve usar o servidor Web [HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente conhecido como [WebListener](xref:fundamentals/servers/weblistener) para aplicativos do ASP.NET Core 1.x) em vez do [Kestrel](xref:fundamentals/servers/kestrel). O IIS é recomendado para uso como um servidor proxy reverso com o Kestrel para implantações de borda. Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Introdução

Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.

1. Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Faça as seguintes alterações em `Program.Main`:

   * Chame `host.RunAsService` em vez de `host.Run`.

   * Se o código chama `UseContentRoot`, use um caminho para o local de publicação em vez de `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Publique o aplicativo em uma pasta. Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica para uma pasta.

4. Teste criando e iniciando o serviço.

   Abra um shell de comando com privilégios administrativos para usar a ferramenta de linha de comando [sc.exe](https://technet.microsoft.com/library/bb490995) para criar e iniciar um serviço. Se o serviço é nomeado MyService, publicado para `c:\svc` e denominado AspNetCoreService, os comandos são:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   O valor `binPath` é o caminho para o executável do aplicativo, que inclui o nome do arquivo executável.

   ![Exemplo de criação e início de janela do console](windows-service/_static/create-start.png)

   Quando terminar desses comandos, navegue até o mesmo caminho usado ao executar como um aplicativo de console (por padrão, `http://localhost:5000`):

   ![Executar em um serviço](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornecer uma maneira de executar fora de um serviço

É mais fácil testar e depurar ao executar fora de um serviço, então é comum adicionar código que chama `RunAsService` apenas em determinadas condições. Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um argumento de linha de comando `--console` ou se o depurador está anexado:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Manipular eventos de início e de parada

Para manipular eventos `OnStarting`, `OnStarted` e `OnStopping`, faça as seguintes alterações adicionais:

1. Crie uma classe que deriva de `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Criar um método de extensão para `IWebHost` que passa o `WebHostService` personalizado para `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Em `Program.Main`, chame o novo método de extensão, `RunAsCustomService`, em vez de `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Se o código `WebHostService` personalizado requer um serviço de injeção de dependência (como um agente), obtenha-o da propriedade `Services` de `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Servidor proxy e cenários de balanceador de carga

Serviços que interagem com solicitações da Internet ou de uma rede corporativa e estão atrás de um proxy ou de um balanceador de carga podem exigir configuração adicional. Para obter mais informações, veja [Configurar o ASP.NET Core para trabalhar com servidores proxy e balanceadores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Agradecimentos

Este artigo foi escrito com a ajuda das fontes publicadas:

* [Hospedar o ASP.NET Core como um serviço Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Como hospedar o ASP.NET Core em um serviço Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
