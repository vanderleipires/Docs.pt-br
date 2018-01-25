---
title: "Visão geral de APIs do consumidor"
author: rick-anderson
description: "Este documento fornece uma visão geral do consumidor várias APIs disponíveis da biblioteca de proteção de dados do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 2545226314ebf57d7a0d644d8edfb5354dcc6e5e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="consumer-apis-overview"></a>Visão geral APIs do consumidor

O `IDataProtectionProvider` e `IDataProtector` interfaces são as interfaces básicas por meio do qual os consumidores usam o sistema de proteção de dados. Eles estão localizados no [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) pacote.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

A interface de provedor representa a raiz do sistema de proteção de dados. Ele não pode ser usado diretamente para proteger ou desproteger dados. Em vez disso, o consumidor deve obter uma referência a um `IDataProtector` chamando `IDataProtectionProvider.CreateProtector(purpose)`, onde o objetivo é uma cadeia de caracteres que descreve o caso de uso pretendido do consumidor. Consulte [cadeias de caracteres de finalidade](purpose-strings.md) para obter mais informações sobre a intenção deste parâmetro e como escolher um valor apropriado.

## <a name="idataprotector"></a>IDataProtector

A interface de protetor é retornada por uma chamada para `CreateProtector`e ele essa interface que os consumidores podem usar para executar proteger e Desproteger as operações.

Para proteger uma parte dos dados, transmitir os dados para o `Protect` método. A interface básica define um método que byte converte [] -> byte [], mas há também uma sobrecarga (fornecida como um método de extensão), que converte a cadeia de caracteres -> a cadeia de caracteres. A segurança oferecida pelos dois métodos é idêntica; o desenvolvedor deve escolher qualquer sobrecarga é mais conveniente para seu caso de uso. Independentemente da sobrecarga escolhida, o valor retornado pelo Proteja método agora está protegido (enciphered e à prova de adulteração) e o aplicativo pode enviá-lo para um cliente não confiável.

Para desproteger uma parte anteriormente protegido dos dados, passar os dados protegidos para o `Unprotect` método. (Há byte []-sobrecargas com base e baseada em cadeia de caracteres para conveniência do desenvolvedor.) Se a carga protegida foi gerada por uma chamada anterior para `Protect` esse mesmo `IDataProtector`, o `Unprotect` método retornará a carga desprotegida original. Se a carga protegida tenha sido violada ou foi produzida por outro `IDataProtector`, o `Unprotect` método lançará CryptographicException.

O conceito de mesmo versus diferentes `IDataProtector` vínculos de volta para o conceito de finalidade. Se dois `IDataProtector` instâncias foram geradas a partir da mesma raiz `IDataProtectionProvider` mas por meio de cadeias de caracteres de finalidade diferente na chamada para `IDataProtectionProvider.CreateProtector`, em seguida, elas são consideradas [protetores diferentes](purpose-strings.md), e um não será possível desproteger cargas geradas por outros.

## <a name="consuming-these-interfaces"></a>Essas interfaces de consumo

Para um componente com reconhecimento de injeção de dependência, o uso pretendido é que o componente levar uma `IDataProtectionProvider` parâmetro em seu construtor e que o sistema DI fornece automaticamente esse serviço quando o componente é instanciado.

> [!NOTE]
> Alguns aplicativos (como aplicativos de console ou aplicativos do ASP.NET 4. x) podem não ser DI com reconhecimento de forma não é possível usar o mecanismo descrito aqui. Para esses cenários consulte o [cenários com reconhecimento de DI não](../configuration/non-di-scenarios.md) documento para obter mais informações sobre a obtenção de uma instância de um `IDataProtection` provedor sem passar por DI.

O exemplo a seguir demonstra três conceitos:

1. [Adicionando o sistema de proteção de dados](../configuration/overview.md) ao contêiner de serviço,

2. Usando DI para receber uma instância de um `IDataProtectionProvider`, e

3. Criando um `IDataProtector` de um `IDataProtectionProvider` e usá-lo para proteger e Desproteger dados.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

O pacote Microsoft.AspNetCore.DataProtection.Abstractions contém um método de extensão `IServiceProvider.GetDataProtector` como uma conveniência de desenvolvedor. Ele encapsula como uma única operação ambos recuperando uma `IDataProtectionProvider` do provedor de serviços e chamar `IDataProtectionProvider.CreateProtector`. O exemplo a seguir demonstra o uso.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instâncias do `IDataProtectionProvider` e `IDataProtector` são thread-safe para chamadores vários. Ele foi desenvolvido que depois que um componente obtém uma referência a um `IDataProtector` por meio de uma chamada para `CreateProtector`, ele usará essa referência para várias chamadas para `Protect` e `Unprotect`. Uma chamada para `Unprotect` lançará CryptographicException se a carga protegida não pode ser verificada ou decifrada. Alguns componentes poderá ignorar erros durante Desproteger operações; um componente que lê os cookies de autenticação pode manipular esse erro e tratar a solicitação, como não se tivesse nenhum cookie de em vez de falhar a solicitação. Componentes que deseja que esse comportamento especificamente devem capturar CryptographicException em vez de assimilação todas as exceções.
