---
title: "Host em um serviço do Windows"
author: tdykstra
description: "Saiba como hospedar um aplicativo ASP.NET Core em um serviço do Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Hospedar um aplicativo ASP.NET Core em um serviço do Windows

Por [Tom Dykstra](https://github.com/tdykstra)

A maneira recomendada para hospedar um aplicativo ASP.NET Core no Windows sem usar o IIS é executá-lo uma [serviço Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Dessa forma ele pode iniciar automaticamente após a reinicialização e falhas, sem esperar que alguém fazer logon.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample)). Consulte o [próximas etapas](#next-steps) seção para obter instruções sobre como executá-lo.

## <a name="prerequisites"></a>Pré-requisitos

* O aplicativo deve ser executado em tempo de execução do .NET Framework.  No *. csproj* de arquivos, especifique os valores apropriados para [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) e [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Veja um exemplo:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Ao criar um projeto no Visual Studio, use o **aplicativo do ASP.NET Core (.NET Framework)** modelo.

* Se o aplicativo recebe solicitações de Internet (não apenas a partir de uma rede interna), ele deve usar o [WebListener](xref:fundamentals/servers/weblistener) servidor web em vez de [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel deve ser usado com o IIS para implantações de borda.  Para obter mais informações, consulte [Quando usar Kestrel com um proxy reverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Introdução

Esta seção explica as alterações mínimas necessárias para configurar um projeto existente do ASP.NET Core para executar em um serviço.

* Instale o pacote NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Faça as seguintes alterações em `Program.Main`:
  
  * Chamar `host.RunAsService` em vez de `host.Run`.
  
  * Se o código chama `UseContentRoot`, use um caminho para o local de publicação em vez de`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Publica o aplicativo em uma pasta.

  Use [dotnet publicar](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) ou um [perfil de publicação do Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica uma pasta.

* Teste ao criar e iniciar o serviço.

  Abra uma janela de prompt de comando de administrador para usar o [sc.exe](https://technet.microsoft.com/library/bb490995) ferramenta de linha de comando para criar e iniciar um serviço.  
  
  Se o serviço é chamado MyService, publicar o aplicativo para `c:\svc`e o aplicativo em si é denominado AspNetCoreService, os comandos teria esta aparência:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  O `binPath` valor é o caminho para o executável do aplicativo, incluindo o nome do arquivo executável em si.

  ![Janela de console criar e iniciar o exemplo](windows-service/_static/create-start.png)

  Quando terminar desses comandos, navegue até o mesmo caminho que durante a execução como um aplicativo de console (por padrão, `http://localhost:5000`)

  ![Executando em um serviço](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Fornecem uma maneira de executar fora de um serviço

É mais fácil de testar e depurar quando em execução fora de um serviço, portanto, é comum para adicionar o código que chama `host.RunAsService` apenas em determinadas condições.  Por exemplo, o aplicativo pode ser executado como um aplicativo de console com um `--console` argumento de linha de comando ou se o depurador é anexado.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Tratar parar e iniciar eventos

Para tratar `OnStarting`, `OnStarted`, e `OnStopping` eventos, faça as seguintes alterações adicionais:

* Crie uma classe que deriva de `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Criar um método de extensão para `IWebHost` que passa personalizado `WebHostService` para `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* Em `Program.Main` alteração chamar o novo método de extensão, em vez de `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Se personalizado `WebHostService` código precisa obter um serviço de injeção de dependência (como um agente de log), obtenha-o do `Services` propriedade `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Próximas etapas

O [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) que acompanha este artigo é um aplicativo da web MVC simples que tenha sido modificado, conforme mostrado no anterior exemplos de código.  Para executá-lo em um serviço, siga as etapas a seguir:

* Publicar *c:\svc*.

* Abra uma janela de administrador.

* Digite os seguintes comandos:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * Em um navegador, vá para http://localhost:5000/ para verificar se ele está em execução.

Se o aplicativo não for iniciado como o esperado quando em execução em um serviço, uma maneira rápida para disponibilizar as mensagens de erro é adicionar um provedor de log, como o [provedor de log de eventos do Windows](xref:fundamentals/logging/index#eventlog).

## <a name="acknowledgments"></a>Confirmações

Este artigo foi escrito com a Ajuda de fontes que já foram publicadas. A primeira e mais úteis, eles foram estes:

* [Hospedagem ASP.NET Core como serviço do Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Como hospedar o ASP.NET Core em um serviço do Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
