---
title: Cadeias de caracteres de finalidade
author: rick-anderson
description: "Este documento detalha como cadeias de caracteres de finalidade são usadas na proteção de dados do ASP.NET Core APIs."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: b1e95c9d0aa8195aa73fddfb97a4079e67a351bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-strings"></a>Cadeias de caracteres de finalidade

<a name="data-protection-consumer-apis-purposes"></a>

Componentes que consomem `IDataProtectionProvider` deve passar em uma única *fins* parâmetro para o `CreateProtector` método. A finalidade *parâmetro* é inerente à segurança do sistema de proteção de dados, como ele fornece isolamento entre consumidores de criptografia, mesmo se as chaves de criptografia de raiz são as mesmas.

Quando um consumidor Especifica a finalidade, a cadeia de caracteres de finalidade é usada junto com as chaves de criptografia de raiz para derivar criptográficas subchaves exclusivas para que o consumidor. Isso isola o consumidor de todos os outros consumidores criptográficos do aplicativo: nenhum outro componente pode ler suas cargas de e não pode ler cargas do qualquer outro componente. Esse isolamento também processa inviável categorias inteiras de ataque em relação ao componente.

![Exemplo de diagrama de finalidade](purpose-strings/_static/purposes.png)

No diagrama acima, `IDataProtector` instâncias A e B **não é possível** ler umas das outras cargas, somente seus próprios.

A cadeia de caracteres de fim não precisa ser segredo. Ele simplesmente deve ser exclusivo no sentido de que nenhum outro componente com bom comportamento já fornecem a mesma cadeia de caracteres de finalidade.

>[!TIP]
> Usando o nome de namespace e o tipo do componente consumindo as APIs de proteção de dados é uma boa regra geral, como prática que essas informações nunca entrarão em conflito.
>
>Um componente de autoria do Contoso que é responsável por minting tokens de portador pode usar Contoso.Security.BearerToken como cadeia de caracteres sua finalidade. Ou - ainda melhor - ele pode usar Contoso.Security.BearerToken.v1 como cadeia de caracteres sua finalidade. Anexar o número de versão permite que uma versão futura usar Contoso.Security.BearerToken.v2 como sua finalidade e as versões diferentes seria completamente isoladas uma da outra quanto cargas ir.

Desde o parâmetro fins `CreateProtector` é uma matriz de cadeia de caracteres, acima podem ter sido em vez disso, especificadas como `[ "Contoso.Security.BearerToken", "v1" ]`. Isso permite o estabelecimento de uma hierarquia de propósitos e abre a possibilidade de cenários de multilocação com o sistema de proteção de dados.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Componentes não devem permitir a entrada do usuário não confiável ser a única origem de entrada para a cadeia de finalidades.
>
>Por exemplo, considere um componente Contoso.Messaging.SecureMessage que é responsável por armazenar mensagens seguras. Se o componente de mensagens seguro chamar `CreateProtector([ username ])`, em seguida, um usuário mal-intencionado pode criar uma conta com o nome de usuário "Contoso.Security.BearerToken" em uma tentativa de obter o componente para chamar `CreateProtector([ "Contoso.Security.BearerToken" ])`, inadvertidamente provocando mensagens seguras sistema cargas Menta que pode ser considerada como tokens de autenticação.
>
>Uma cadeia de fins melhor para o componente de mensagens seria `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, que fornece isolamento apropriado.

O isolamento fornecido pelos e comportamentos de `IDataProtectionProvider`, `IDataProtector`, e fins é o seguinte:

* Para um determinado `IDataProtectionProvider` objeto, o `CreateProtector` método criará uma `IDataProtector` objeto vinculado exclusivamente para ambos os `IDataProtectionProvider` objeto que criou e o parâmetro de fins que foi passado para o método.

* O parâmetro de fim não deve ser nulo. (Se fins é especificado como uma matriz, isso significa que a matriz não deve ser de comprimento zero e todos os elementos da matriz devem ser não-nulo.) Uma finalidade de cadeia de caracteres vazia é tecnicamente permitida, mas não é recomendada.

* Argumentos de duas finalidades são equivalentes se e somente se eles contêm as mesmas cadeias de caracteres (usando um comparador ordinal) na mesma ordem. Um argumento de finalidade única é equivalente à matriz de elemento único fins correspondente.

* Dois `IDataProtector` objetos forem equivalentes, se e somente se eles são criados a partir equivalente `IDataProtectionProvider` objetos com parâmetros de fins equivalente.

* Para um determinado `IDataProtector` objeto, uma chamada para `Unprotect(protectedData)` retornará original `unprotectedData` se e somente se `protectedData := Protect(unprotectedData)` para um equivalente `IDataProtector` objeto.

> [!NOTE]
> Não estamos considerando o caso onde algum componente intencionalmente escolhe uma cadeia de caracteres de finalidade que é conhecida em conflito com outro componente. Esse componente essencialmente seria considerado mal-intencionados e este sistema não serve para fornecer garantias de segurança que um código mal-intencionado já está em execução dentro do processo de trabalho.
