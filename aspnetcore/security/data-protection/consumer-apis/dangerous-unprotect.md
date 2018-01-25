---
title: "Desproteção cargas cujas chaves foram revogados"
author: rick-anderson
description: "Este documento explica como Desproteger dados protegidos com as chaves que já tiverem sido revogadas, em um aplicativo do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 08a8ad9b3b3cc2de48751d4149bf39c58954fd90
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Desproteção cargas cujas chaves foram revogados

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

A proteção de dados do ASP.NET Core APIs não são principalmente para persistência indefinida de cargas confidenciais. Outras tecnologias, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) são mais adequados para o cenário de armazenamento indefinido, e eles têm recursos de gerenciamento de chave forte de forma correspondente. Dito isso, não há nada proibindo um desenvolvedor usa as APIs de proteção de dados ASP.NET Core para proteção de longo prazo de dados confidenciais. Chaves nunca são removidas do anel chave, portanto `IDataProtector.Unprotect` sempre pode recuperar conteúdos existentes desde que as chaves estão disponíveis e válido.

No entanto, um problema surge quando o desenvolvedor tenta Desproteger dados que foi protegidos com uma chave revogada, como `IDataProtector.Unprotect` lançará uma exceção, nesse caso. Isso pode ser bom para cargas de curta duração ou transitórias (como tokens de autenticação), pois esses tipos de cargas podem ser facilmente recriados pelo sistema e na pior das hipóteses, o visitante do site pode ser necessário para fazer logon novamente. Mas para cargas persistentes, tendo `Unprotect` throw pode levar a perda inaceitável de dados.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para suportar o cenário de permitir que conteúdos a ser desprotegido mesmo em face chaves revogadas, o sistema de proteção de dados contém um `IPersistedDataProtector` tipo. Para obter uma instância de `IPersistedDataProtector`, simplesmente obter uma instância de `IDataProtector` no modo normal e tente converter o `IDataProtector` para `IPersistedDataProtector`.

> [!NOTE]
> Nem todos os `IDataProtector` instâncias podem ser convertidas em `IPersistedDataProtector`. Os desenvolvedores devem usar o c# como operador ou semelhante evitar exceções de tempo de execução causado por conversões inválidos e devem estar preparados para tratar o caso de falha de maneira adequada.

`IPersistedDataProtector`expõe a superfície de API a seguir:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Essa API usa a carga protegida (como uma matriz de bytes) e retorna a carga desprotegida. Não há nenhuma sobrecarga baseada em cadeia de caracteres. Os dois parâmetros de saída são da seguinte maneira.

* `requiresMigration`: será definido como true se a chave usada para proteger essa carga não é mais a chave padrão ativo, por exemplo, a chave usada para proteger essa carga é antiga e tem uma chave sem interrupção operação desde tomado local. O chamador poderá proteger novamente a carga dependendo de suas necessidades de negócios.

* `wasRevoked`: será definido como verdadeiro se a chave usada para proteger essa carga foi revogada.

>[!WARNING]
> Tenha bastante cuidado ao passar `ignoreRevocationErrors: true` para o `DangerousUnprotect` método. Se, depois de chamar esse método de `wasRevoked` valor for true, em seguida, a chave usada para proteger essa carga foi revogada e autenticidade da carga deve ser tratada como suspeito. Nesse caso, apenas continue a operar na carga de desprotegidos se você tiver alguma garantia separada que é autêntica, por exemplo, se ele é proveniente de um banco de dados seguro em vez de sendo enviada por um cliente da web não confiável.

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
