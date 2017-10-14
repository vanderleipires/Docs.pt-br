---
title: Formato de armazenamento de chaves
author: tdykstra
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 832f150b269e36ac35f0d00cafbf4903f7aef4fc
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="key-storage-format"></a>Formato de armazenamento de chaves

<a name="data-protection-implementation-key-storage-format"></a>

Objetos são armazenados em repouso na representação XML. O diretório padrão para armazenamento de chaves é % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>O \<chave > elemento

Chaves existirem como objetos de nível superior no repositório de chave. Por convenção, as chaves têm o nome do arquivo **chave-{guid}. XML**, onde {guid} é a id da chave. Cada arquivo contém uma única chave. O formato do arquivo é da seguinte maneira.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

O \<chave > elemento contém os seguintes atributos e elementos filho:

* A id de chave. Esse valor é tratado como autoritativo; o nome do arquivo é simplesmente uma sutileza para facilitar a leitura humana.

* A versão do \<chave > elemento fixado no momento em 1.

* Datas de expiração, ativação e criação da chave.

* Um \<descritor > elemento, que contém informações sobre a implementação de criptografia autenticada contida dentro dessa chave.

No exemplo acima, id da chave é {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, ele foi criado e ativado em 19 de março de 2015, e ele tem um tempo de vida de 90 dias. (Ocasionalmente, a data de ativação pode ser um pouco antes da data de criação, como neste exemplo. Isso ocorre devido a um nit em como as APIs de trabalho e é inofensivas na prática.)

## <a name="the-descriptor-element"></a>O \<descritor > elemento

Externa \<descritor > elemento contém um deserializerType de atributo, que é o nome qualificado do assembly de um tipo que implementa IAuthenticatedEncryptorDescriptorDeserializer. Esse tipo é responsável por ler interna \<descritor > elemento e para analisar as informações contidas em.

O formato específico do \<descritor > elemento depende da implementação do Criptografador autenticado encapsulada pela chave e cada tipo desserializador espera um formato ligeiramente diferente para isso. Em geral, no entanto, esse elemento conterá informações de algoritmos (nomes, tipos, OIDs, ou semelhante) e o material de chave secreta. No exemplo acima, o descritor especifica que esta chave encapsula a criptografia AES-256-CBC + HMACSHA256 validação.

## <a name="the-encryptedsecret-element"></a>O \<encryptedSecret > elemento

Um <encryptedSecret> elemento que contém o formulário criptografado do material de chave secreto pode estar presente se [criptografia de segredos em repouso está ativada](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). O atributo decryptorType será o nome qualificado do assembly de um tipo que implementa IXmlDecryptor. Esse tipo é responsável por ler interna <encryptedKey> elemento e descriptografá-los para recuperar o texto não criptografado original.

Assim como acontece com \<descritor >, o formato específico do <encryptedSecret> elemento depende do mecanismo de criptografia em repouso em uso. No exemplo acima, a chave mestra é criptografada usando a DPAPI do Windows por comentário.

## <a name="the-revocation-element"></a>O \<revogação > elemento

Revogações existem como objetos de nível superior no repositório de chave. Por convenção revogações tem o nome do arquivo **. XML de revogação-{timestamp}** (para revogar todas as chaves antes de uma data específica) ou **revogação-{guid}. XML** (para revogar uma chave específica). Cada arquivo contém um único \<revogação > elemento.

Para revogações de chaves individuais, o conteúdo do arquivo será abaixo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Nesse caso, somente a chave especificada é revogada. Se a id de chave é "*", no entanto, como no exemplo abaixo, todas as chaves cuja data de criação está antes da data de revogação especificada serão revogadas.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

O \<motivo > elemento nunca é lidos pelo sistema. Ele é simplesmente um local conveniente para armazenar um motivo legível revogação.
