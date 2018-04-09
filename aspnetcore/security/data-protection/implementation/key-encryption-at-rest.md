---
title: Criptografia de chave em repouso no núcleo do ASP.NET
author: rick-anderson
description: Saiba os detalhes da implementação de criptografia de chave de proteção de dados do ASP.NET Core em repouso.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 9247b141a44c958f34529e5a42a0ddc8c8893cb0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Criptografia de chave em repouso no núcleo do ASP.NET

<a name="data-protection-implementation-key-encryption-at-rest"></a>

Por padrão, o sistema de proteção de dados [emprega uma heurística](xref:security/data-protection/configuration/default-settings) para determinar o material de chave de criptografia como devem ser criptografados em repouso. O desenvolvedor pode substituir a heurística e especificar manualmente como chaves devem ser criptografadas em repouso.

> [!NOTE]
> Se você especificar uma criptografia de chave explícita no mecanismo de rest, o sistema de proteção de dados irá cancelar o registro o mecanismo de armazenamento de chave padrão que a heurística fornecida. Você deve [especificar um mecanismo de armazenamento de chave explícita](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), caso contrário, o sistema de proteção de dados não será iniciado.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

O sistema de proteção de dados é fornecido com três mecanismos de criptografia de chave na caixa.

## <a name="windows-dpapi"></a>Windows DPAPI

*Esse mecanismo está disponível apenas no Windows.*

Quando o Windows DPAPI é usado, material de chave será criptografada por meio de [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) antes de ser persistidos para armazenamento. A DPAPI é um mecanismo de criptografia apropriadas para os dados que nunca serão lidas fora do computador atual (no entanto é possível fazer essas chaves até do Active Directory; Consulte [DPAPI e perfis móveis](https://support.microsoft.com/kb/309408/#6)). Por exemplo configurar a criptografia de chave em repouso DPAPI.

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Se `ProtectKeysWithDpapi` é chamado sem parâmetros, somente a conta de usuário atual do Windows pode decifrar o material da chave persistente. Opcionalmente, você pode especificar que qualquer conta de usuário no computador (não apenas a conta de usuário atual) deve ser capaz de decifrar o material da chave, conforme mostrado no exemplo abaixo.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>Certificado x. 509

*Esse mecanismo não está disponível em `.NET Core 1.0` ou `1.1`.*

Se seu aplicativo é distribuído em vários computadores, pode ser conveniente para distribuir um certificado x. 509 compartilhado entre as máquinas e configurar aplicativos para usar este certificado para criptografia de chaves em repouso. Veja abaixo um exemplo.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

Devido às limitações do .NET Framework somente os certificados com chaves particulares CAPI têm suporte. Consulte [criptografia baseada em certificado com o Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) abaixo para obter possíveis soluções para essas limitações.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Esse mecanismo está disponível somente no Windows 8 / Windows Server 2012 e posterior.*

Começando com o Windows 8, o sistema operacional oferece suporte a DPAPI-NG (também chamado de CNG DPAPI). Microsoft dispõe seu cenário de uso da seguinte maneira.

   Computação em nuvem, no entanto, geralmente exige que conteúdo criptografado em um computador ser descriptografado em outro. Portanto, começando com o Windows 8, estendido a ideia de usar uma API relativamente simples para abranger os cenários de nuvem da Microsoft. Essa nova API chamada DPAPI-NG, permite que você compartilhe com segurança segredos (chaves, senhas, material de chave) e mensagens protegendo-os para um conjunto de entidades que podem ser usados para desprotegê-los em diferentes computadores após a autorização e autenticação adequada.

   De [sobre DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

A entidade de segurança é codificada como uma regra de proteção do descritor. Considere o exemplo abaixo, que criptografa material de chave, de modo que somente o usuário associado a um domínio com o SID especificado pode descriptografar o material da chave.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

Também há uma sobrecarga sem parâmetros de `ProtectKeysWithDpapiNG`. Este é um método conveniente para especificar a regra "SID = meu", onde meu é o SID da conta de usuário do Windows atual.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

Nesse cenário, o controlador de domínio do AD é responsável por distribuir as chaves de criptografia usadas pelas operações NG DPAPI. O usuário de destino poderá decifrar a carga criptografada de qualquer computador ingressado no domínio (desde que o processo está sendo executado sob sua identidade).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Criptografia baseada em certificado com o Windows DPAPI-NG

Se você estiver executando no Windows 8.1 / Windows Server 2012 R2 ou posterior, você pode usar Windows DPAPI-NG para executar a criptografia baseada em certificado, mesmo se o aplicativo estiver em execução no .NET Core. Para tirar proveito disso, use a cadeia de caracteres do descritor de regra "certificado = HashId:thumbprint", onde a impressão digital é codificada em hexadecimal SHA1 impressão digital do certificado a ser usado. Veja abaixo um exemplo.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Qualquer aplicativo que está apontado para este repositório deve estar em execução no Windows 8.1 / Windows Server 2012 R2 ou posterior para ser capaz de decifrar essa chave.

## <a name="custom-key-encryption"></a>Criptografia de chave personalizada

Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de criptografia de chave, fornecendo um personalizado `IXmlEncryptor`.
