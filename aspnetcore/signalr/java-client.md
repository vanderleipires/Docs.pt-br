---
title: Cliente de Java do SignalR Core ASP.NET
author: mikaelm12
description: Saiba como usar o cliente de Java de SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 646118c78d5d38b44b89d399cd06a5332a11d064
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207765"
---
# <a name="aspnet-core-signalr-java-client"></a>Cliente de Java do SignalR Core ASP.NET

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

O cliente de Java permite se conectar a um servidor do SignalR do ASP.NET Core do código Java, incluindo aplicativos Android. Como o [cliente JavaScript](xref:signalr/javascript-client) e o [cliente .NET](xref:signalr/dotnet-client), o cliente de Java permite que você receber e enviar mensagens a um hub em tempo real. O cliente de Java está disponível no ASP.NET Core 2.2 e posterior.

O aplicativo de console do Java de exemplo referenciado neste artigo usa o cliente de Java do SignalR.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([como baixar](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instalar o pacote de cliente Java do SignalR

O *signalr 1.0.0-preview3 35501* arquivo JAR permite que os clientes se conectem aos hubs de SignalR. Para localizar o número de versão de arquivo JAR mais recente, consulte o [resultados da pesquisa Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Se usando o Gradle, adicione a seguinte linha para o `dependencies` seção do seu *Build. gradle* arquivo:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Se usando o Maven, adicione as seguintes linhas dentro de `<dependencies>` elemento da sua *POM. XML* arquivo:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Conectar a um hub

Para estabelecer uma `HubConnection`, o `HubConnectionBuilder` deve ser usado. O nível de log e a URL do hub pode ser configurado durante a criação de uma conexão. Configurar as opções necessárias chamando o `HubConnectionBuilder` métodos antes `build`. Iniciar a conexão com `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Chamar métodos de hub do cliente

Uma chamada para `send` invoca um método de hub. Passe o nome do método de hub e quaisquer argumentos definidos no método de hub para `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Chamar métodos de cliente do hub

Use `hubConnection.on` para definir métodos no cliente que o hub pode chamar. Defina os métodos depois de criar, mas antes de iniciar a conexão.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Adicionar registro em log

O cliente SignalR Java usa a [SLF4J](https://www.slf4j.org/) biblioteca para registro em log. É uma API de alto nível de log que permite aos usuários da biblioteca de escolher sua própria implementação de log específico, colocando em uma dependência de log específico. O trecho de código a seguir mostra como usar `java.util.logging` com o cliente de Java do SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Se você não configurar o registro em log em suas dependências, SLF4J carrega um agente de operação não padrão com a seguinte mensagem de aviso:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Isso pode ser ignorado.

## <a name="known-limitations"></a>Limitações conhecidas

Esta é uma versão de visualização do cliente Java. Não há suporte para alguns recursos:

* Há suporte para apenas o protocolo JSON.
* Há suporte para apenas o transporte de WebSockets.
* Streaming ainda não tem suporte.

## <a name="additional-resources"></a>Recursos adicionais

* [Referência de API Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
