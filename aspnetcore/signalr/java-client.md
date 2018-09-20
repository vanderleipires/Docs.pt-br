---
title: Cliente de Java do SignalR Core ASP.NET
author: mikaelm12
description: Saiba como usar o cliente de Java de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482912"
---
# <a name="aspnet-core-signalr-java-client"></a>Cliente de Java do SignalR do ASP.NET Core

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

O cliente de Java permite se conectar a um servidor do SignalR do ASP.NET Core do código Java, incluindo aplicativos Android. Como o [cliente JavaScript](xref:signalr/javascript-client) e o [cliente .NET](xref:signalr/dotnet-client), o cliente de Java permite que você receber e enviar mensagens a um hub em tempo real. O cliente de Java está disponível no ASP.NET Core 2.2 e posterior.

O aplicativo de console do Java de exemplo referenciado neste artigo usa o cliente de Java do SignalR.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instalar o pacote de cliente Java do SignalR

O *signalr 0.1.0 – visualização 2 35174* arquivo JAR permite que os clientes se conectem aos hubs de SignalR. Para localizar o número de versão de arquivo JAR mais recente, consulte o [resultados da pesquisa Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Se usando o Gradle, adicione a seguinte linha para o `dependencies` seção do seu *Build. gradle* arquivo:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Se usando o Maven, adicione as seguintes linhas dentro de `<dependencies>` elemento da sua *POM. XML* arquivo:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Conectar a um hub

Para estabelecer uma `HubConnection`, o `HubConnectionBuilder` deve ser usado. O nível de log e a URL do hub pode ser configurado durante a criação de uma conexão. Configurar as opções necessárias chamando o `HubConnectionBuilder` métodos antes `build`. Iniciar a conexão com `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

Uma chamada para `send` invoca um método de hub. Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Use `hubConnection.on` para definir métodos no cliente que o hub pode chamar. Defina os métodos depois de criar, mas antes de iniciar a conexão.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Limitações conhecidas

Isso é uma versão de visualização inicial do cliente Java. Há muitos recursos que ainda não têm suporte. Os intervalos a seguir estão sendo trabalhados para versões futuras:

* Apenas tipos primitivos podem ser aceitos como parâmetros e tipos de retorno.
* As APIs são síncronas.
* Somente o tipo de chamada de "Envio" é suportado neste momento. "Invocar" e transmissão de valores de retorno não são suportados.
* Há suporte para apenas o protocolo JSON.
* Há suporte para apenas o transporte de WebSockets.

## <a name="additional-resources"></a>Recursos adicionais

* [Referência da API Java](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
