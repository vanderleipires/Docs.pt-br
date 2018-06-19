---
title: Começar com as APIs de proteção de dados no ASP.NET Core
author: rick-anderson
description: Saiba como usar as APIs de proteção de dados ASP.NET Core para proteger e ao desproteger dados em um aplicativo.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 3a69abd2b58e02f87ccaf2317b0a8a2a7e9d7b4a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076982"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Começar com as APIs de proteção de dados no ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Em seus dados mais simples, protegendo consiste as seguintes etapas:

1. Crie um tipo de dados protetor de um provedor de proteção de dados.

2. Chamar o `Protect` método com os dados que você deseja proteger.

3. Chamar o `Unprotect` método com os dados que você deseja transformar novamente em texto sem formatação.

A maioria das estruturas e modelos de aplicativo, como ASP.NET ou SignalR, já que configurar o sistema de proteção de dados e adicioná-lo para um contêiner de serviço, que você pode acessar por meio de injeção de dependência. O exemplo a seguir demonstra como configurar um contêiner de serviço para injeção de dependência e registrando a pilha de proteção de dados, recebendo o provedor de proteção de dados por meio de DI, criando um protetor e desproteção e proteção de dados

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando você cria um protetor você deve fornecer um ou mais [cadeias de caracteres de finalidade](xref:security/data-protection/consumer-apis/purpose-strings). Uma cadeia de caracteres de finalidade fornece isolamento entre os consumidores. Por exemplo, um protetor criado com uma cadeia de caracteres de fim de "green" não será possível desproteger dados fornecidos por um protetor com a finalidade de "roxo".

>[!TIP]
> Instâncias do `IDataProtectionProvider` e `IDataProtector` são thread-safe para chamadores vários. Ele foi desenvolvido que depois que um componente obtém uma referência a um `IDataProtector` por meio de uma chamada para `CreateProtector`, ele usará essa referência para várias chamadas para `Protect` e `Unprotect`.
>
>Uma chamada para `Unprotect` lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada. Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação. Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.
