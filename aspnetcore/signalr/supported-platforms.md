---
title: Plataformas com suporte do SignalR do ASP.NET Core
author: tdykstra
description: Saiba mais sobre as plataformas com suporte para o SignalR do ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758174"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Plataformas com suporte do SignalR do ASP.NET Core

## <a name="server-system-requirements"></a>Requisitos de sistema do servidor

SignalR para ASP.NET Core dá suporte a qualquer plataforma de servidor que dá suporte ao ASP.NET Core.

## <a name="javascript-client"></a>Cliente JavaScript

O [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) é executado no NodeJS 8 e versões posteriores e os seguintes navegadores:

| Navegador                         | Versão |
| ------------------------------- | ------- |
| Microsoft Edge                  | atual |
| Mozilla Firefox                 | atual |
| Google Chrome; inclui o Android | atual |
| Safari; inclui o iOS            | atual |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Cliente .NET

O [cliente .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) é executado em qualquer plataforma de servidor com suporte pelo ASP.NET Core.

Se o servidor executa o IIS, o transporte de WebSockets requer o IIS 8.0 ou superior no Windows Server 2012 ou superior. Outros transportes têm suporte em todas as plataformas.

## <a name="java-client"></a>Cliente de Java

O [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) dá suporte a Java 8 e versões posteriores.

## <a name="unsupported-clients"></a>Não há suporte para clientes

Os clientes a seguir estão disponíveis, mas são experimentais ou não oficial. Eles não têm suporte no momento e nunca podem ser.

* [Cliente C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Cliente SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
