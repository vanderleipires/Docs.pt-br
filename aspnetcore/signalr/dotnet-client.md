---
title: Cliente de .NET do SignalR do ASP.NET Core
author: tdykstra
description: Informações sobre o cliente do .NET Core ASP.NET SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373313"
---
# <a name="aspnet-core-signalr-net-client"></a>Cliente de .NET do SignalR do ASP.NET Core

Por [Rachel Appel](http://twitter.com/rachelappel)

A biblioteca de cliente .NET de SignalR do ASP.NET Core permite que você se comunicar com os hubs de SignalR em aplicativos .NET.

> [!NOTE]
> Xamarin tem requisitos especiais para a versão do Visual Studio. Para obter mais informações, consulte [cliente SignalR 2.1.1 no Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente .NET de SignalR do ASP.NET Core.

## <a name="install-the-signalr-net-client-package"></a>Instalar o pacote de cliente .NET do SignalR

O `Microsoft.AspNetCore.SignalR.Client` pacote é necessário para clientes .NET conectar-se aos hubs de SignalR. Para instalar a biblioteca de cliente, execute o seguinte comando na **Package Manager Console** janela:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Conectar a um hub

Para estabelecer uma conexão, cria uma `HubConnectionBuilder` e chamar `Build`. A URL do hub, protocolo, o tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão. Configurar as opções necessárias, inserindo qualquer um dos `HubConnectionBuilder` métodos em `Build`. Iniciar a conexão com `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

`InvokeAsync` chama métodos no hub. Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `InvokeAsync`. O SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Definir métodos de hub de chamadas usando `connection.On` depois de criar, mas antes de iniciar a conexão.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

O código anterior no `connection.On` é executado quando o código do lado do servidor chama-o usando o `SendAsync` método.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Registro em log e tratamento de erros

Tratar erros com uma instrução try-catch. Inspecione o `Exception` objeto para determinar a ação apropriada a tomar depois de ocorrer um erro.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Recursos adicionais

* [Hubs](xref:signalr/hubs)
* [Cliente JavaScript](xref:signalr/javascript-client)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)
