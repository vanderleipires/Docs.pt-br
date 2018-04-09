---
title: Derivação subchave e criptografia autenticada no núcleo do ASP.NET
author: rick-anderson
description: Saiba os detalhes de implementação de proteção de dados do ASP.NET Core derivação de subchaves e autenticado criptografia.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 8c83da40a524896becc07c94c01d5e2b684e4386
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Derivação subchave e criptografia autenticada no núcleo do ASP.NET

<a name="data-protection-implementation-subkey-derivation"></a>

A maioria das chaves do anel de chave contém alguma forma de entropia e terá informações algorítmicos informando "criptografia de modo CBC + validação HMAC" ou "criptografia GCM + validação". Nesses casos, nos referimos a entropia inserida como o material de chave mestre (ou KM) para essa chave e podemos executar uma função de derivação de chave para derivar as chaves que serão usadas para operações de criptografia reais.

> [!NOTE]
> As chaves são abstratas e uma implementação personalizada pode não se comportar como abaixo. Se a chave fornece sua própria implementação do `IAuthenticatedEncryptor` em vez de usar uma das nossas fábricas internas, o mecanismo descrito nesta seção não se aplica.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Dados autenticados adicionais e subchave derivação

O `IAuthenticatedEncryptor` interface serve como a interface principal para todas as operações de criptografia autenticada. Seu `Encrypt` método usa dois buffers: texto sem formatação e additionalAuthenticatedData (AAD). O fluxo de conteúdo de texto sem formatação inalterado a chamada para `IDataProtector.Protect`, mas o AAD é gerado pelo sistema e consiste em três componentes:

1. 32 bits magic cabeçalho 09 F0 C9 F0 que identifica esta versão do sistema de proteção de dados.

2. A id de chave de 128 bits.

3. Uma cadeia de caracteres de comprimento variável formada da cadeia de finalidade que criou o `IDataProtector` que está executando esta operação.

Como o AAD é exclusivo para a tupla de todos os três componentes, podemos usá-lo derivar novas chaves KM em vez de usar KM em si em todos os nossos operações criptográficas. Para todas as chamadas para `IAuthenticatedEncryptor.Encrypt`, ocorre o processo de derivação de chaves a seguir:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

Aqui, estamos ligando para o NIST SP800-108 KDF no modo de contador (consulte [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), SEC 5.1) com os seguintes parâmetros:

* Chave de derivação de chave (KDK) = K_M

* PRF = HMACSHA512

* label = additionalAuthenticatedData

* context = contextHeader || keyModifier

O cabeçalho de contexto é de comprimento variável e essencialmente serve como uma impressão digital dos algoritmos para a qual estamos estiver derivando K_E e K_H. O modificador de chave é uma cadeia de caracteres de 128 bits gerada aleatoriamente para cada chamada `Encrypt` e serve para garantir com grandes probabilidade que KE e KH são exclusivas para essa operação de criptografia de autenticação específico, mesmo se todas as outras entradas para o KDF é constante.

Para criptografia de modo CBC + operações de validação de HMAC, | K_E | é o comprimento da chave de criptografia simétrica bloco, e | K_H | é o tamanho do resumo da rotina de HMAC. Para criptografia GCM + operações de validação, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Criptografia de modo CBC + validação HMAC

Depois de K_E é gerado por meio do mecanismo acima, podemos gerar um vetor de inicialização aleatória e executar o algoritmo de criptografia simétrica de bloco para codificar o texto não criptografado. O vetor de inicialização e o texto cifrado são, em seguida, executar a rotina HMAC inicializada com a chave K_H para produzir Mac. Esse processo e o valor de retorno é representado graficamente abaixo.

![Processo do modo CBC e retorno](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> O `IDataProtector.Protect` implementação será [preceda o cabeçalho magic e a id de chave](xref:security/data-protection/implementation/authenticated-encryption-details) a saída antes de retorná-lo ao chamador. Porque o cabeçalho magic e a id de chave são implicitamente parte do [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), e porque o modificador de chave é passado como entrada para o KDF, isso significa que cada byte único da carga final retornada é autenticado pelo Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>Criptografia de modo Galois/contador + validação

Quando K_E é gerado por meio do mecanismo acima, podemos gerar um nonce de 96 bits aleatório e executar o algoritmo de criptografia simétrica de bloco para codificar o texto não criptografado e produzir a marca de autenticação de 128 bits.

![Processo de modo GCM e de retorno](subkeyderivation/_static/galoisprocess.png)

*saída: = keyModifier | | nonce | | E_gcm (dados de uso único, K_E,) | | authTag*

> [!NOTE]
> Embora GCM nativamente dá suporte ao conceito de AAD, podemos estiver ainda de alimentação AAD somente para o KDF original, aceitar para passar uma cadeia de caracteres vazia para GCM para seu parâmetro AAD. A razão para isso é dupla. Primeiro, [para dar suporte a agilidade](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) nunca queremos usar K_M diretamente como a chave de criptografia. Além disso, GCM impõe requisitos de exclusividade muito estrita em suas entradas. Define a probabilidade de que a rotina de criptografia do GCM é sempre invocado em dois ou mais distintos de dados de entrada com o mesmo (chave, nonce) par não deve exceder 2 ^ 32. Se corrigir K_E não foi possível realizar mais de 2 ^ 32 operações de criptografia antes de podemos executar afoul da 2 ^ limitar -32. Isso pode parecer um grande número de operações, mas um servidor web de alto tráfego pode passar por solicitações de 4 bilhões em alguns dias, dentro do tempo de vida normal para essas chaves. Para manter a conformidade de 2 ^ limite de probabilidade-32, podemos continuar a usar um modificador de chave de 128 bits e nonce de 96 bits, o que aumenta a contagem de operação utilizável para qualquer K_M determinado. Para manter a simplicidade de design compartilha o caminho do código KDF entre as operações CBC e GCM e como AAD já é considerado o KDF não é necessário encaminhá-lo para a rotina do GCM.
