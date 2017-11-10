---
title: "Guia de Introdução com as APIs de proteção de dados"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Guia de Introdução com as APIs de proteção de dados

<a name="security-data-protection-getting-started"></a>

Em seus dados mais simples de proteção consiste as seguintes etapas:

1. Crie um tipo de dados protetor de um provedor de proteção de dados.

2. Chame o método de proteção com os dados que você deseja proteger.

3. Chame o método Unprotect com os dados que você deseja transformar novamente em texto sem formatação.

A maioria das estruturas, como ASP.NET ou SignalR já configurar o sistema de proteção de dados e adicioná-lo para um contêiner de serviço, que você pode acessar por meio de injeção de dependência. O exemplo a seguir demonstra como configurar um contêiner de serviço para injeção de dependência e registrando a pilha de proteção de dados, recebendo o provedor de proteção de dados por meio de DI, criando um protetor e desproteção e proteção de dados

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Quando você cria um protetor você deve fornecer um ou mais [cadeias de caracteres de finalidade](consumer-apis/purpose-strings.md). Uma cadeia de caracteres de finalidade fornece isolamento entre os consumidores, por exemplo um protetor criado com uma cadeia de caracteres de fim de "green" não poderá desproteger dados fornecidos por um protetor com a finalidade de "roxo".

>[!TIP]
> Instâncias de IDataProtectionProvider e IDataProtector são thread-safe para chamadores vários. Destina-se quando um componente obtém uma referência a um IDataProtector por meio de uma chamada para CreateProtector, ele usará essa referência para várias chamadas para proteger e Desproteger.
>
>Uma chamada para desproteger lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada. Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação. Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.
