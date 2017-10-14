---
title: "Desproteção cargas cujas chaves foram revogados"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 082fd69769dd0ef000b39ec148c12719d66f7aac
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Desproteção cargas cujas chaves foram revogados

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

A proteção de dados do ASP.NET Core APIs não são principalmente para persistência indefinida de cargas confidenciais. Outras tecnologias, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) são mais adequados para o cenário de armazenamento indefinido, e eles têm recursos de gerenciamento de chave forte de forma correspondente. Dito isso, não há nada proibindo um desenvolvedor usa as APIs de proteção de dados ASP.NET Core para proteção de longo prazo de dados confidenciais. Chaves nunca são removidas do anel de chave, portanto IDataProtector.Unprotect sempre pode recuperar cargas existentes, desde que as chaves estão disponíveis e válido.

No entanto, um problema surge quando o desenvolvedor tenta Desproteger dados que foi protegidos com uma chave revogada, como IDataProtector.Unprotect lançará uma exceção nesse caso. Isso pode ser bom para cargas de curta duração ou transitórias (como tokens de autenticação), pois esses tipos de cargas podem ser facilmente recriados pelo sistema e na pior das hipóteses, o visitante do site pode ser necessário para fazer logon novamente. Mas cargas persistente, ter Desproteger throw pode levar a perda inaceitável de dados.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para suportar o cenário de permitir que conteúdos a ser desprotegido mesmo em face chaves revogadas, o sistema de proteção de dados contém um tipo de IPersistedDataProtector. Para obter uma instância de IPersistedDataProtector, obter uma instância de IDataProtector da maneira normal e tente converter IDataProtector para IPersistedDataProtector.

> [!NOTE]
> Nem todas as instâncias de IDataProtector podem ser convertidas em IPersistedDataProtector. Os desenvolvedores devem usar o c# como operador ou semelhante evitar exceções de tempo de execução causado por conversões inválidos e devem estar preparados para tratar o caso de falha de maneira adequada.

IPersistedDataProtector expõe a superfície de API a seguir:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

Essa API usa a carga protegida (como uma matriz de bytes) e retorna a carga desprotegida. Não há nenhuma sobrecarga baseada em cadeia de caracteres. Os dois parâmetros de saída são da seguinte maneira.

* requiresMigration: será definido como true se a chave usada para proteger essa carga não é mais a chave padrão ativo, por exemplo, a chave usada para proteger essa carga é antiga e tem uma chave sem interrupção operação desde tomado local. O chamador poderá proteger novamente a carga dependendo de suas necessidades de negócios.

* wasRevoked: será definido como verdadeiro se a chave usada para proteger essa carga foi revogada.

>[!WARNING]
> Tenha bastante cuidado ao passar ignoreRevocationErrors: True para o método DangerousUnprotect. Se depois de chamar esse método, o valor de wasRevoked for true, em seguida, a chave usada para proteger essa carga foi revogada e autenticidade da carga deve ser tratada como suspeito. Nesse caso somente continuar a operar na carga de desprotegidos se você tiver alguma garantia separada que é autêntica, por exemplo, que ele do provenientes de um banco de dados seguro em vez de sendo enviada por um cliente da web não confiável.

[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
