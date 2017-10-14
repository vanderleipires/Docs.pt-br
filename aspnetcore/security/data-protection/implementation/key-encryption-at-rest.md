---
title: Criptografia de chave em repouso
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f2bbbf4e-0945-43ce-be59-8bf19e448798
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 5d0eb4036a3d491336cbe9357779c150b5cbb236
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="key-encryption-at-rest"></a><span data-ttu-id="50ee1-103">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="50ee1-103">Key Encryption At Rest</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="50ee1-104">Por padrão, o sistema de proteção de dados [emprega uma heurística](../configuration/default-settings.md#data-protection-default-settings) para determinar o material de chave de criptografia como devem ser criptografados em repouso.</span><span class="sxs-lookup"><span data-stu-id="50ee1-104">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="50ee1-105">O desenvolvedor pode substituir a heurística e especificar manualmente como chaves devem ser criptografadas em repouso.</span><span class="sxs-lookup"><span data-stu-id="50ee1-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="50ee1-106">Se você especificar uma criptografia de chave explícita no mecanismo de rest, o sistema de proteção de dados irá cancelar o registro o mecanismo de armazenamento de chave padrão que a heurística fornecida.</span><span class="sxs-lookup"><span data-stu-id="50ee1-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="50ee1-107">Você deve [especificar um mecanismo de armazenamento de chave explícita](key-storage-providers.md#data-protection-implementation-key-storage-providers), caso contrário, o sistema de proteção de dados não será iniciado.</span><span class="sxs-lookup"><span data-stu-id="50ee1-107">You must [specify an explicit key storage mechanism](key-storage-providers.md#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="50ee1-108">O sistema de proteção de dados é fornecido com três mecanismos de criptografia de chave na caixa.</span><span class="sxs-lookup"><span data-stu-id="50ee1-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="50ee1-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="50ee1-109">Windows DPAPI</span></span>

<span data-ttu-id="50ee1-110">*Esse mecanismo está disponível apenas no Windows.*</span><span class="sxs-lookup"><span data-stu-id="50ee1-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="50ee1-111">Quando o Windows DPAPI é usado, material de chave será criptografada por meio de [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) antes de ser persistidos para armazenamento.</span><span class="sxs-lookup"><span data-stu-id="50ee1-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="50ee1-112">A DPAPI é um mecanismo de criptografia apropriadas para os dados que nunca serão lidas fora do computador atual (no entanto é possível fazer essas chaves até do Active Directory; Consulte [DPAPI e perfis móveis](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="50ee1-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it is possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="50ee1-113">Por exemplo configurar a criptografia de chave em repouso DPAPI.</span><span class="sxs-lookup"><span data-stu-id="50ee1-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
       // only the local user account can decrypt the keys
       .ProtectKeysWithDpapi();
   ```

<span data-ttu-id="50ee1-114">Se ProtectKeysWithDpapi for chamado sem parâmetros, somente a Windows conta de usuário atual pode decifrar o material da chave persistente.</span><span class="sxs-lookup"><span data-stu-id="50ee1-114">If ProtectKeysWithDpapi is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="50ee1-115">Opcionalmente, você pode especificar que qualquer conta de usuário no computador (não apenas a conta de usuário atual) deve ser capaz de decifrar o material da chave, conforme mostrado no exemplo abaixo.</span><span class="sxs-lookup"><span data-stu-id="50ee1-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
       // all user accounts on the machine can decrypt the keys
       .ProtectKeysWithDpapi(protectToLocalMachine: true);
   ```

## <a name="x509-certificate"></a><span data-ttu-id="50ee1-116">Certificado x. 509</span><span class="sxs-lookup"><span data-stu-id="50ee1-116">X.509 certificate</span></span>

<span data-ttu-id="50ee1-117">*Esse mecanismo não está disponível em `.NET Core 1.0` ou `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="50ee1-117">*This mechanism is not available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="50ee1-118">Se seu aplicativo é distribuído em vários computadores, pode ser conveniente para distribuir um certificado x. 509 compartilhado entre as máquinas e configurar aplicativos para usar este certificado para criptografia de chaves em repouso.</span><span class="sxs-lookup"><span data-stu-id="50ee1-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="50ee1-119">Veja abaixo um exemplo.</span><span class="sxs-lookup"><span data-stu-id="50ee1-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
   ```

<span data-ttu-id="50ee1-120">Devido às limitações do .NET Framework somente os certificados com chaves particulares CAPI têm suporte.</span><span class="sxs-lookup"><span data-stu-id="50ee1-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="50ee1-121">Consulte [criptografia baseada em certificado com o Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) abaixo para obter possíveis soluções para essas limitações.</span><span class="sxs-lookup"><span data-stu-id="50ee1-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="50ee1-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="50ee1-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="50ee1-123">*Esse mecanismo está disponível somente no Windows 8 / Windows Server 2012 e posterior.*</span><span class="sxs-lookup"><span data-stu-id="50ee1-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="50ee1-124">Começando com o Windows 8, o sistema operacional oferece suporte a DPAPI-NG (também chamado de CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="50ee1-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="50ee1-125">Microsoft dispõe seu cenário de uso da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="50ee1-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="50ee1-126">Computação em nuvem, no entanto, geralmente exige que conteúdo criptografado em um computador ser descriptografado em outro.</span><span class="sxs-lookup"><span data-stu-id="50ee1-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="50ee1-127">Portanto, começando com o Windows 8, estendido a ideia de usar uma API relativamente simples para abranger os cenários de nuvem da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="50ee1-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="50ee1-128">Essa nova API chamada DPAPI-NG, permite que você compartilhe com segurança segredos (chaves, senhas, material de chave) e mensagens protegendo-os para um conjunto de entidades que podem ser usados para desprotegê-los em diferentes computadores após a autorização e autenticação adequada.</span><span class="sxs-lookup"><span data-stu-id="50ee1-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="50ee1-129">De [sobre DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="50ee1-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="50ee1-130">A entidade de segurança é codificada como uma regra de proteção do descritor.</span><span class="sxs-lookup"><span data-stu-id="50ee1-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="50ee1-131">Considere o exemplo abaixo, que criptografa material de chave, de modo que somente o usuário associado a um domínio com o SID especificado pode descriptografar o material da chave.</span><span class="sxs-lookup"><span data-stu-id="50ee1-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID=S-1-5-21-..."
     .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
       flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="50ee1-132">Também há uma sobrecarga sem parâmetros de ProtectKeysWithDpapiNG.</span><span class="sxs-lookup"><span data-stu-id="50ee1-132">There is also a parameterless overload of ProtectKeysWithDpapiNG.</span></span> <span data-ttu-id="50ee1-133">Este é um método conveniente para especificar a regra "SID = meu", onde meu é o SID da conta de usuário do Windows atual.</span><span class="sxs-lookup"><span data-stu-id="50ee1-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
     // uses the descriptor rule "SID={current account SID}"
     .ProtectKeysWithDpapiNG();
   ```

<span data-ttu-id="50ee1-134">Nesse cenário, o controlador de domínio do AD é responsável por distribuir as chaves de criptografia usadas pelas operações NG DPAPI.</span><span class="sxs-lookup"><span data-stu-id="50ee1-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="50ee1-135">O usuário de destino poderá decifrar a carga criptografada de qualquer computador ingressado no domínio (desde que o processo está sendo executado sob sua identidade).</span><span class="sxs-lookup"><span data-stu-id="50ee1-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="50ee1-136">Criptografia baseada em certificado com o Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="50ee1-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="50ee1-137">Se você estiver executando no Windows 8.1 / Windows Server 2012 R2 ou posterior, você pode usar Windows DPAPI-NG para executar a criptografia baseada em certificado, mesmo que o aplicativo está em execução no [.NET Core](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="50ee1-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on [.NET Core](https://www.microsoft.com/net/core).</span></span> <span data-ttu-id="50ee1-138">Para tirar proveito disso, use a cadeia de caracteres do descritor de regra "certificado = HashId:thumbprint", onde a impressão digital é codificada em hexadecimal SHA1 impressão digital do certificado a ser usado.</span><span class="sxs-lookup"><span data-stu-id="50ee1-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="50ee1-139">Veja abaixo um exemplo.</span><span class="sxs-lookup"><span data-stu-id="50ee1-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
       // searches the cert store for the cert with this thumbprint
       .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
           flags: DpapiNGProtectionDescriptorFlags.None);
   ```

<span data-ttu-id="50ee1-140">Qualquer aplicativo que está apontado para este repositório deve estar em execução no Windows 8.1 / Windows Server 2012 R2 ou posterior para ser capaz de decifrar essa chave.</span><span class="sxs-lookup"><span data-stu-id="50ee1-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="50ee1-141">Criptografia de chave personalizada</span><span class="sxs-lookup"><span data-stu-id="50ee1-141">Custom key encryption</span></span>

<span data-ttu-id="50ee1-142">Se os mecanismos de caixa de entrada não forem apropriados, o desenvolvedor pode especificar seu próprio mecanismo de criptografia de chave, fornecendo um IXmlEncryptor personalizado.</span><span class="sxs-lookup"><span data-stu-id="50ee1-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom IXmlEncryptor.</span></span>
