---
title: "Visão geral de APIs do consumidor"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>Visão geral APIs do consumidor

As interfaces IDataProtectionProvider e IDataProtector são as interfaces básicas por meio do qual os consumidores usam o sistema de proteção de dados. Eles estão localizados no pacote Microsoft.AspNetCore.DataProtection.Abstractions.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

A interface de provedor representa a raiz do sistema de proteção de dados. Ele não pode ser usado diretamente para proteger ou desproteger dados. Em vez disso, o consumidor deve obter uma referência a um IDataProtector chamando IDataProtectionProvider.CreateProtector(purpose), onde o objetivo é uma cadeia de caracteres que descreve o caso de uso pretendido do consumidor. Consulte [cadeias de caracteres de finalidade](purpose-strings.md) para obter mais informações sobre a intenção deste parâmetro e como escolher um valor apropriado.

## <a name="idataprotector"></a>IDataProtector

A interface de protetor é retornada por uma chamada para CreateProtector e é essa interface que os consumidores podem usar para executar proteger e Desproteger as operações.

Para proteger uma parte dos dados, passe os dados para o método de proteção. A interface básica define um método que byte converte [] -> byte [], mas também há uma sobrecarga (fornecida como um método de extensão), que converte a cadeia de caracteres -> a cadeia de caracteres. A segurança oferecida pelos dois métodos é idêntica; o desenvolvedor deve escolher qualquer sobrecarga é mais conveniente para seu caso de uso. Independentemente da sobrecarga escolhida, o valor retornado pelo Proteja método agora está protegido (enciphered e à prova de adulteração) e o aplicativo pode enviá-lo para um cliente não confiável.

Para desproteger uma parte anteriormente protegido dos dados, passe os dados protegidos para o método Desproteger. (Há byte []-sobrecargas com base e baseada em cadeia de caracteres para conveniência do desenvolvedor.) Se a carga protegida foi gerada por uma chamada anterior para proteger nesse mesmo IDataProtector, o método Unprotect retornará a carga desprotegida original. Se a carga protegida tenha sido violada ou foi gerada por um IDataProtector diferente, o método Desproteger lançará CryptographicException.

Vincula o conceito de mesmo versus IDataProtector diferente para o conceito de finalidade. Se duas instâncias de IDataProtector foram geradas da mesma raiz IDataProtectionProvider mas por meio de cadeias de caracteres de finalidade diferente na chamada para IDataProtectionProvider.CreateProtector, eles são considerados [protetores diferentes](purpose-strings.md), e um não será possível desproteger cargas, geradas por outros.

## <a name="consuming-these-interfaces"></a>Essas interfaces de consumo

Para um componente com reconhecimento de injeção de dependência, o uso pretendido é que o componente de ter um parâmetro IDataProtectionProvider em seu construtor e que o sistema DI fornece automaticamente esse serviço quando o componente é instanciado.

> [!NOTE]
> Alguns aplicativos (como aplicativos de console ou aplicativos do ASP.NET 4. x) podem não ser DI com reconhecimento de forma não é possível usar o mecanismo descrito aqui. Para esses cenários consulte o [cenários com reconhecimento de DI não](../configuration/non-di-scenarios.md) documento para obter mais informações sobre a obtenção de uma instância de um provedor de IDataProtection sem passar por DI.

O exemplo a seguir demonstra três conceitos:

1. [Adicionando o sistema de proteção de dados](../configuration/overview.md) ao contêiner de serviço,

2. Usando a injeção de dependência para receber uma instância de um IDataProtectionProvider, e

3. Criando um IDataProtector de um IDataProtectionProvider e usá-lo para proteger e Desproteger dados.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

O pacote Microsoft.AspNetCore.DataProtection.Abstractions contém um método de extensão IServiceProvider.GetDataProtector como uma conveniência de desenvolvedor. Ele encapsula como uma única operação tanto recuperando um IDataProtectionProvider do provedor de serviço e chamando IDataProtectionProvider.CreateProtector. O exemplo a seguir demonstra o uso.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instâncias de IDataProtectionProvider e IDataProtector são thread-safe para chamadores vários. Ele destina-se quando um componente obtém uma referência a um IDataProtector por meio de uma chamada para CreateProtector, ele usará essa referência para várias chamadas para proteger e chamada Unprotect.A para desproteger lançará CryptographicException se a carga protegida não pode ser verificado ou decifradas. Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação. Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.
