---
title: "Cabeçalhos de contexto"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 7befd983f6a45839868639708ec5cf45bf2df35f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="context-headers"></a>Cabeçalhos de contexto

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a>Plano de fundo e a teoria

No sistema de proteção de dados, uma "chave" significa que um objeto que pode fornecer serviços de criptografia de autenticação. Cada chave é identificado por uma id exclusiva (uma GUID) e transporta informações algorítmicos e material entropic. Ele destina-se que cada chave realizar entropia exclusiva, mas o sistema não pode impor que e precisamos para desenvolvedores que podem alterar manualmente o anel de chave, modificando as informações de algoritmos de uma chave existente do anel de chave de conta. Para alcançar nossos requisitos de segurança fornecidos nesses casos, o sistema de proteção de dados tem um conceito de [agilidade criptográfica](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045), que permite a com segurança usando um único valor entropic entre vários algoritmos de criptografia.

A maioria dos sistemas que oferecem suporte a agilidade criptográfica de fazer isso, incluindo algumas informações de identificação sobre o algoritmo de carga. OID do algoritmo geralmente é uma boa candidata para isso. No entanto, um problema que tivemos é que há várias maneiras de especificar o mesmo algoritmo: "AES" (CNG) e o gerenciado Aes, AesManaged, AesCryptoServiceProvider, AesCng e RijndaelManaged (fornecida parâmetros específicos) classes são, na verdade, todos os mesmos coisa e é necessário manter um mapeamento de tudo isso para a identificação de objeto correta. Se um desenvolvedor deseja fornecer um algoritmo personalizado (ou até mesmo outra implementação do AES!), eles precisam Conte-nos sua OID. Essa etapa de registro extra facilita a configuração do sistema particularmente penoso.

Recuando, decidimos que podemos foram chegando o problema de direção errada. Um OID informa o que é o algoritmo, mas, na verdade, não importa sobre isso. Se for necessário usar um único valor entropic com segurança em dois algoritmos diferentes, não é necessário para que possamos saber o que os algoritmos realmente são. O que é realmente importante é como eles se comportam. Qualquer algoritmo de codificação de bloco simétrica razoável também é uma forte permutação pseudoaleatórios (PRP): corrija as entradas (chave, cadeia de texto sem formatação de modo, o IV,) e a saída de texto cifrado com grandes probabilidade serão diferente dos outros qualquer codificação de bloco simétrica algoritmo considerando as entradas do mesmo. Da mesma forma, qualquer função de hash com chave razoável também é uma função pseudoaleatórios forte (PRF) e fornecido um conjunto fixo de entrada sua saída predominantemente serão diferente de qualquer outra função de hash com chave.

Podemos usar esse conceito de alta segurança PRPs e PRFs para criar um cabeçalho de contexto. Esse cabeçalho de contexto atua essencialmente como uma impressão digital estável sobre os algoritmos em uso para qualquer operação especificada e fornece a agilidade criptográfica necessária ao sistema de proteção de dados. Esse cabeçalho pode ser reproduzido e é usado posteriormente como parte do [processo de derivação de subchave](subkeyderivation.md#data-protection-implementation-subkey-derivation). Há duas maneiras diferentes para criar o cabeçalho de contexto dependendo os modos de operação dos algoritmos subjacentes.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Criptografia de modo CBC + autenticação HMAC

<a name=data-protection-implementation-context-headers-cbc-components></a>

O cabeçalho de contexto consiste dos seguintes componentes:

* [16 bits] O valor 00 00, o que é um marcador que significa "criptografia CBC + autenticação HMAC".

* [32 bits] O comprimento da chave (em bytes, big-endian) do algoritmo de criptografia simétrica de bloco.

* [32 bits] O tamanho de bloco (em bytes, big-endian) do algoritmo de criptografia simétrica de bloco.

* [32 bits] O comprimento da chave (em bytes, big-endian) do algoritmo HMAC. (Atualmente o tamanho da chave sempre coincide com o tamanho do resumo.)

* [32 bits] O tamanho (em bytes, big-endian) resumo do algoritmo HMAC.

* EncCBC (K_E, IV, ""), que é a saída do algoritmo de criptografia simétrica bloco recebe uma entrada de cadeia de caracteres vazia e onde o IV é um vetor de zeros. A construção de K_E é descrita abaixo.

* MAC (K_H, ""), que é a saída do algoritmo HMAC recebe uma entrada de cadeia de caracteres vazia. A construção de K_H é descrita abaixo.

O ideal é poderíamos passar vetores de zeros para K_E e K_H. No entanto, queremos evitar a situação em que o algoritmo subjacente verifica a existência de baixa segurança chaves antes de executar quaisquer operações (especialmente DES e 3DES), que impede usando um padrão simple ou repeatable como um vetor de zeros.

Em vez disso, usamos o NIST SP800-108 KDF no modo de contador (consulte [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), SEC 5.1) com uma chave de comprimento zero, o rótulo e o contexto e a HMACSHA512 como o PRF subjacente. Podemos derivar | K_E | + | K_H | bytes de saída, em seguida, decompor o resultado em K_E e K_H próprios. Matematicamente, isso é representado como a seguir.

(K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, chave = "", rótulo = "", contexto = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Exemplo: AES de 192 de CBC + HMACSHA256

Por exemplo, considere o caso em que o algoritmo de criptografia simétrica de bloco é AES de 192 de CBC e o algoritmo de validação é HMACSHA256. O sistema poderia gerar o cabeçalho de contexto usando as etapas a seguir.

Primeiro, permitem (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, chave = "", rótulo = "", contexto = ""), onde | K_E | = 192 bits e | K_H | = 256 bits por algoritmos especificados. Isso leva a K_E = 5BB6. 21DD e K_H = A04A. 00A9 no exemplo a seguir:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

Em seguida, calcular Enc_CBC (K_E, IV, "") para AES-192-CBC fornecido IV = 0 * e K_E como acima.

resultado: = F474B1872B3B53E4721DE19C0841DB6F

Em seguida, o MAC de computação (K_H, "") para HMACSHA256 determinado K_H como acima.

resultado: = D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Isso gera o cabeçalho de contexto completo abaixo:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

Esse cabeçalho de contexto é a impressão digital do par de algoritmo de criptografia autenticada (criptografia AES de 192 de CBC + HMACSHA256 validação). Os componentes, conforme descrito [acima](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) são:

* o marcador (00 00)

* o comprimento da chave de codificação de bloco (00 00 00 18)

* o tamanho de bloco de codificação de bloco (00 00 00 10)

* o comprimento da chave HMAC (00 00 00 20)

* o tamanho do resumo HMAC (00 00 00 20)

* a codificação de bloco saída PRP (F4 74 - DB 6F) e

* a saída PRF HMAC (D4 79 - end).

> [!NOTE]
> A criptografia de modo CBC + HMAC cabeçalho de contexto de autenticação baseia-se da mesma maneira, independentemente de se as implementações de algoritmos são fornecidas pelo Windows CNG ou por tipos SymmetricAlgorithm e KeyedHashAlgorithm gerenciados. Isso permite que os aplicativos executados em diferentes sistemas operacionais confiável produzem o mesmo cabeçalho de contexto, embora as implementações de algoritmos diferem entre sistemas operacionais. (Na prática, o KeyedHashAlgorithm não precisa ser um HMAC adequado. Ele pode ser qualquer tipo de algoritmo de hash com chave.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Exemplo: 3DES-192-CBC + HMACSHA1

Primeiro, permitem (K_E | | K_H) = SP800_108_CTR (prf = HMACSHA512, chave = "", rótulo = "", contexto = ""), onde | K_E | = 192 bits e | K_H | = 160 bits por algoritmos especificados. Isso leva a K_E = A219. E2BB e K_H = DC4A. B464 no exemplo a seguir:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

Em seguida, calcular Enc_CBC (K_E, IV, "") para 3DES-192-CBC fornecido IV = 0 * e K_E como acima.

resultado: = ABB100F81E53E10E

Em seguida, o MAC de computação (K_H, "") para HMACSHA1 determinado K_H como acima.

resultado: = 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Isso gera o cabeçalho de contexto completo que é uma impressão digital do autenticado par de algoritmo do encryption (criptografia 3DES-192-CBC + HMACSHA1 validação), mostrado abaixo:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

Os componentes são divididos da seguinte maneira:

* o marcador (00 00)

* o comprimento da chave de codificação de bloco (00 00 00 18)

* o tamanho de bloco de codificação de bloco (00 00 00 08)

* o comprimento da chave HMAC (00 00 00 14)

* o tamanho do resumo HMAC (00 00 00 14)

* a codificação de bloco saída PRP (B1 AB - E1 0E) e

* a saída PRF HMAC (76 EB - end).

## <a name="galoiscounter-mode-encryption--authentication"></a>Criptografia de modo Galois/contador + autenticação

O cabeçalho de contexto consiste dos seguintes componentes:

* [16 bits] O valor 00 01, que é um marcador que significa "criptografia GCM + autenticação".

* [32 bits] O comprimento da chave (em bytes, big-endian) do algoritmo de criptografia simétrica de bloco.

* [32 bits] O tamanho nonce (em bytes, big-endian) usado durante as operações de criptografia autenticada. (Para nosso sistema, isso é fixo em tamanho nonce = 96 bits.)

* [32 bits] O tamanho de bloco (em bytes, big-endian) do algoritmo de criptografia simétrica de bloco. (Para GCM, isso é fixo em tamanho de bloco = 128 bits.)

* [32 bits] O autenticação marca tamanho (em bytes, big-endian) produzido pela função criptografia autenticada. (Para nosso sistema, isso é fixo em tamanho de marca = 128 bits.)

* [128 bits] A marca de Enc_GCM (K_E, nonce, ""), que é a saída do algoritmo de criptografia simétrica bloco recebe uma entrada de cadeia de caracteres vazia e onde nonce é um vetor de zeros de 96 bits.

K_E é obtida usando o mesmo mecanismo como a criptografia CBC + o cenário de autenticação de HMAC. No entanto, já que não há nenhum K_H em jogo aqui, essencialmente temos | K_H | = 0, e o algoritmo recolhe para o formulário abaixo.

K_E = SP800_108_CTR (prf = HMACSHA512, chave = "", rótulo = "", contexto = "")

### <a name="example-aes-256-gcm"></a>Exemplo: AES-256-GCM

Primeiramente, vamos K_E = SP800_108_CTR (prf = HMACSHA512, chave = "", rótulo = "", contexto = ""), onde | K_E | = 256 bits.

K_E: = 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Em seguida, calcular a marca de autenticação de Enc_GCM (K_E, nonce, "") para AES-256-GCM fornecido nonce = 096 e K_E como acima.

resultado: = E7DCCE66DF855A323A6BB7BD7A59BE45

Isso gera o cabeçalho de contexto completo abaixo:

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

Os componentes são divididos da seguinte maneira:

   * o marcador (00 01)

   * o comprimento da chave de codificação de bloco (00 00 00 20)

   * o tamanho de nonce (00 00 00 0 C)

   * o tamanho de bloco de codificação de bloco (00 00 00 10)

   * o tamanho de marca de autenticação (00 00 00 10) e

   * a marca de autenticação da execução de codificação de bloco (controlador de domínio E7 - end).
