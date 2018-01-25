---
title: "Guia de Introdução com as APIs de proteção de dados"
author: rick-anderson
description: "Este documento explica como usar as APIs de proteção de dados ASP.NET Core para proteger e ao desproteger dados em um aplicativo."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: c6a631b6dc4a7855b11031dfcef42b17906754b0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a>Guia de Introdução com as APIs de proteção de dados

<a name="security-data-protection-getting-started"></a>

Em seus dados mais simples, protegendo consiste as seguintes etapas:

1. Crie um tipo de dados protetor de um provedor de proteção de dados.

2. Chamar o `Protect` método com os dados que você deseja proteger.

3. Chamar o `Unprotect` método com os dados que você deseja transformar novamente em texto sem formatação.

A maioria das estruturas e modelos de aplicativo, como ASP.NET ou SignalR, já que configurar o sistema de proteção de dados e adicioná-lo para um contêiner de serviço, que você pode acessar por meio de injeção de dependência. O exemplo a seguir demonstra como configurar um contêiner de serviço para injeção de dependência e registrando a pilha de proteção de dados, recebendo o provedor de proteção de dados por meio de DI, criando um protetor e desproteção e proteção de dados

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando você cria um protetor você deve fornecer um ou mais [cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md). Uma cadeia de caracteres de finalidade fornece isolamento entre os consumidores. Por exemplo, um protetor criado com uma cadeia de caracteres de fim de "green" não será possível desproteger dados fornecidos por um protetor com a finalidade de "roxo".

>[!TIP]
> Instâncias do `IDataProtectionProvider` e `IDataProtector` são thread-safe para chamadores vários. Ele foi desenvolvido que depois que um componente obtém uma referência a um `IDataProtector` por meio de uma chamada para `CreateProtector`, ele usará essa referência para várias chamadas para `Protect` e `Unprotect`.
>
>Uma chamada para `Unprotect` lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada. Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação. Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.
