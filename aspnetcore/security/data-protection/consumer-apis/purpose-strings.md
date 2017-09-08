---
title: Cadeias de caracteres de finalidade
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: cc33bcfab4945e6d6f9ca7e61edeff4d1837661a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="purpose-strings"></a>Cadeias de caracteres de finalidade

<a name=data-protection-consumer-apis-purposes></a>

Componentes que consomem IDataProtectionProvider devem passar um exclusivo *fins* parâmetro para o método CreateProtector. A finalidade *parâmetro* é inerente à segurança do sistema de proteção de dados, como ele fornece isolamento entre consumidores de criptografia, mesmo se as chaves de criptografia de raiz são as mesmas.

Quando um consumidor Especifica a finalidade, a cadeia de caracteres de finalidade é usada junto com as chaves de criptografia de raiz para derivar criptográficas subchaves exclusivas para que o consumidor. Isso isola o consumidor de todos os outros consumidores criptográficos do aplicativo: nenhum outro componente pode ler suas cargas de e não pode ler cargas do qualquer outro componente. Esse isolamento também processa inviável categorias inteiras de ataque em relação ao componente.

![Exemplo de diagrama de finalidade](purpose-strings/_static/purposes.png)

No diagrama acima IDataProtector instâncias A e B **não é possível** ler umas das outras cargas, somente seus próprios.

A cadeia de caracteres de fim não precisa ser segredo. Ele simplesmente deve ser exclusivo no sentido de que nenhum outro componente com bom comportamento já fornecem a mesma cadeia de caracteres de finalidade.

>[!TIP]
> Usando o nome de namespace e o tipo do componente consumindo as APIs de proteção de dados é uma boa regra geral, como prática que essas informações nunca entrarão em conflito.
>
>Um componente de autoria do Contoso que é responsável por minting tokens de portador pode usar Contoso.Security.BearerToken como cadeia de caracteres sua finalidade. Ou - ainda melhor - ele pode usar Contoso.Security.BearerToken.v1 como cadeia de caracteres sua finalidade. Anexar o número de versão permite que uma versão futura usar Contoso.Security.BearerToken.v2 como sua finalidade e as versões diferentes seria completamente isoladas uma da outra quanto cargas ir.

Como o parâmetro fins CreateProtector é uma matriz de cadeia de caracteres, acima podem ter sido em vez disso, especificadas como ["Contoso.Security.BearerToken", "v1"]. Isso permite o estabelecimento de uma hierarquia de propósitos e abre a possibilidade de cenários de multilocação com o sistema de proteção de dados.

<a name=data-protection-contoso-purpose></a>

>[!WARNING]
> Componentes não devem permitir a entrada do usuário não confiável ser a única origem de entrada para a cadeia de finalidades.
>
>Por exemplo, considere um componente Contoso.Messaging.SecureMessage que é responsável por armazenar mensagens seguras. Se o componente de mensagens seguro foram chamar CreateProtector ([username]), um usuário mal-intencionado pode criar uma conta com o nome de usuário "Contoso.Security.BearerToken" em uma tentativa de obter o componente para chamar CreateProtector([" Contoso.Security.BearerToken"]), assim, inadvertidamente fazendo com que o sistema de mensagens seguro Menta cargas que pode ser considerado como tokens de autenticação.
>
>Uma cadeia de fins melhor para o componente de mensagens seria CreateProtector (["Contoso.Messaging.SecureMessage", "usuário: nome de usuário"]), que fornece isolamento apropriado.

O isolamento fornecido pelos e comportamentos de IDataProtectionProvider, IDataProtector e fins são da seguinte maneira:

* Para um determinado objeto IDataProtectionProvider, o método CreateProtector criará um objeto IDataProtector exclusivamente vinculado ao objeto IDataProtectionProvider que foi criado e o parâmetro de fins que foi passado para o método.

* O parâmetro de fim não deve ser nulo. (Se fins é especificado como uma matriz, isso significa que a matriz não deve ser de comprimento zero e todos os elementos da matriz devem ser não-nulo.) Uma finalidade de cadeia de caracteres vazia é tecnicamente permitida, mas não é recomendada.

* Argumentos de duas finalidades são equivalentes se e somente se eles contêm as mesmas cadeias de caracteres (usando um comparador ordinal) na mesma ordem. Um argumento de finalidade única é equivalente à matriz de elemento único fins correspondente.

* Dois objetos IDataProtector são equivalentes se e somente se eles são criados a partir de objetos equivalentes de IDataProtectionProvider com parâmetros de fins equivalente.

* Para um determinado objeto IDataProtector, uma chamada para Unprotect(protectedData) retornará o unprotectedData original se e somente se protectedData: = Protect(unprotectedData) para um objeto IDataProtector equivalente.

> [!NOTE]
> Não estamos considerando o caso onde algum componente intencionalmente escolhe uma cadeia de caracteres de finalidade que é conhecida em conflito com outro componente. Esse componente essencialmente seria considerado mal-intencionados e este sistema não serve para fornecer garantias de segurança que um código mal-intencionado já está em execução dentro do processo de trabalho.
