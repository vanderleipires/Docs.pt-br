---
title: Cliente de .NET Core ASP.NET SignalR
author: rachelappel
description: Informações sobre o cliente do ASP.NET Core SignalR .NET
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/18/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/dotnet-client
ms.openlocfilehash: 412d2362575789f1fb4792940df6d3dd24dbdd5a
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2018
---
# <a name="aspnet-core-signalr-net-client"></a>Cliente de .NET Core ASP.NET SignalR

Por [Rachel Appel](http://twitter.com/rachelappel)

O cliente do ASP.NET Core SignalR .NET pode ser usado por aplicativos Xamarin, o WPF, o Windows Forms, o Console e o .NET Core. Como o [cliente JavaScript](xref:signalr/javascript-client), o cliente .NET permite que você receber e enviar e receber mensagens para um hub em tempo real.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O exemplo de código neste artigo é um aplicativo do WPF que usa o cliente do ASP.NET Core SignalR .NET.

## <a name="setup-client"></a>Instalar o cliente

O `Microsoft.AspNetCore.SignalR.Client` do pacote é necessário para que os clientes .NET conectar-se a hubs de SignalR. Para instalar a biblioteca de cliente, execute o seguinte comando **Package Manager Console** janela:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Conectar a um hub

Para estabelecer uma conexão, criar um `HubConnectionBuilder` e chame `Build`. A URL do hub, protocolo, tipo de transporte, nível de log, cabeçalhos e outras opções podem ser configuradas durante a criação de uma conexão. Configure as opções necessárias, inserindo um do `HubConnectionBuilder` métodos em `Build`. Iniciar a conexão com `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

`InvokeAsync` chama os métodos no hub. Passe o nome do método de hub e argumentos definidos no método de hub para `InvokeAsync`. SignalR é assíncrono, portanto, use `async` e `await` ao fazer as chamadas.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Definir métodos hub chamadas usando `connection.On` após a criação, mas antes de iniciar a conexão.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

O código anterior no `connection.On` é executado quando o código do lado do servidor chamá-lo usando o `SendAsync` método.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Registro em log e tratamento de erros

Tratar erros com uma instrução try-catch. Inspecione o `Exception` objeto para determinar a ação adequada ocorre após a ocorrência de um erro.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Recursos adicionais

* [Hubs](xref:signalr/hubs)
* [Cliente JavaScript](xref:signalr/javascript-client)
* [Publicar no Azure](xref:signalr/publish-to-azure-web-app)