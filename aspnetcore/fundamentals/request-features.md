---
title: "Recursos de solicitação do ASP.NET Core"
author: ardalis
description: "Saiba mais sobre os detalhes de implementação do servidor web relacionados a solicitações HTTP e respostas que são definidas em interfaces para ASP.NET Core."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a>Recursos de solicitação do ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Os detalhes de implementação de servidor Web relacionados a solicitações HTTP e respostas são definidos em interfaces. Essas interfaces são usadas por implementações de servidor e middleware para criar e modificar o pipeline de hospedagem do aplicativo.

## <a name="feature-interfaces"></a>Interfaces de recurso

ASP.NET Core define um número de interfaces de recurso HTTP em `Microsoft.AspNetCore.Http.Features` que são usados pelos servidores para identificar os recursos que oferecem suporte. As seguintes interfaces de recurso lidar com solicitações e respostas de retorno:

`IHttpRequestFeature`Define a estrutura de uma solicitação HTTP, incluindo protocolo, caminho, cadeia de caracteres de consulta, cabeçalhos e corpo.

`IHttpResponseFeature`Define a estrutura de uma resposta HTTP, incluindo o código de status, cabeçalhos e corpo da resposta.

`IHttpAuthenticationFeature`Define o suporte para identificar os usuários com base em um `ClaimsPrincipal` e especificando um manipulador de autenticação.

`IHttpUpgradeFeature`Define o suporte para [HTTP atualizações](https://tools.ietf.org/html/rfc2616.html#section-14.42), que permitem que o cliente especifique quais adicionais protocolos que ele gostaria de usar se o servidor que deseja alternar protocolos.

`IHttpBufferingFeature`Define métodos para desabilitar o buffer de solicitações de e/ou respostas.

`IHttpConnectionFeature`Define propriedades para portas e endereços locais e remotos.

`IHttpRequestLifetimeFeature`Define o suporte para anular conexões ou detectar se uma solicitação foi encerrada prematuramente, tais como por uma desconexão do cliente.

`IHttpSendFileFeature`Define um método para enviar arquivos de forma assíncrona.

`IHttpWebSocketFeature`Define uma API para soquetes da web de suporte.

`IHttpRequestIdentifierFeature`Adiciona uma propriedade que pode ser implementada para identificar exclusivamente as solicitações.

`ISessionFeature`Define `ISessionFactory` e `ISession` abstrações para dar suporte a sessões de usuário.

`ITlsConnectionFeature`Define uma API para recuperar os certificados de cliente.

`ITlsTokenBindingFeature`Define métodos para trabalhar com parâmetros de token de ligação de TLS.

> [!NOTE]
> `ISessionFeature`não é um recurso de servidor, mas é implementado pelo `SessionMiddleware` (consulte [Gerenciando o estado do aplicativo](app-state.md)).

## <a name="feature-collections"></a>Coleções de recurso

O `Features` propriedade `HttpContext` fornece uma interface para obter e definir os recursos disponíveis de HTTP para a solicitação atual. Como a coleção de recurso é mutável, mesmo dentro do contexto de uma solicitação, o middleware pode ser usado para modificar a coleção e adicionar suporte para recursos adicionais.

## <a name="middleware-and-request-features"></a>Recursos de solicitação e de middleware

Enquanto os servidores são responsáveis por criar a coleção de recursos, middleware pode adicionar a esta coleção e consomem recursos da coleção. Por exemplo, o `StaticFileMiddleware` acessa o `IHttpSendFileFeature` recurso. Se o recurso existir, ele é usado para enviar o arquivo estático solicitado de seu caminho físico. Caso contrário, um método alternativo mais lento será usado para enviar o arquivo. Quando disponível, o `IHttpSendFileFeature` permite que o sistema operacional abrir o arquivo e executar uma cópia de modo kernel direto para a placa de rede.

Além disso, o middleware pode adicionar à coleção de recurso estabelecida pelo servidor. Recursos existentes ainda podem ser substituídos pelo middleware, permitindo que o middleware ampliar a funcionalidade do servidor. Recursos adicionados à coleção estão disponíveis imediatamente para o outro middleware ou o aplicativo em si subjacente posteriormente no pipeline de solicitação.

Combinando implementações personalizadas de servidor e aprimoramentos de middleware específico, o conjunto exato de recursos que requer que um aplicativo pode ser construído. Isso permite que não tem recursos a serem adicionados sem a necessidade de uma alteração no servidor e garante que somente a quantidade mínima de recursos são expostas, limitando assim a ataque de superfície área e melhorar o desempenho.

## <a name="summary"></a>Resumo

Interfaces de recurso Definir recursos específicos de HTTP que pode dar suporte a uma determinada solicitação. Servidores definem coleções de recursos e o conjunto inicial de recursos com suporte pelo servidor, mas middleware pode ser usado para aprimorar a esses recursos.

## <a name="additional-resources"></a>Recursos adicionais

* [Servidores](servers/index.md)

* [Middleware](middleware.md)

* [OWIN (Open Web Interface para .NET)](owin.md)
