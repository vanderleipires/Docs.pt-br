---
title: "Visão geral sobre segurança do ASP.NET Core | Microsoft Docs"
author: rachelappel
description: "Saiba mais sobre conceitos básicos de autenticação, autorização e segurança no ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: f6a1f32c1edd098b0782fd066d8e32f09952a9b7
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="aspnet-core-security-overview"></a>Visão geral sobre segurança do ASP.NET Core

O ASP.NET Core permite que desenvolvedores configurem e gerenciem facilmente a segurança de seus aplicativos. O ASP.NET Core contém recursos para gerenciamento de autenticação, autorização, proteção de dados, imposição de SSL, segredos de aplicativo, proteção contra falsificação de solicitação e gerenciamento de CORS. Esses recursos de segurança permitem que você crie aplicativos de ASP.NET Core robustos e seguros ao mesmo tempo. 

## <a name="aspnet-core-security-features"></a>Recursos de segurança do ASP.NET Core

O ASP.NET Core fornece várias ferramentas e bibliotecas para proteger seus aplicativos, incluindo provedores de identidade internos, mas você pode usar serviços de identidade de terceiros, como Facebook, Twitter e LinkedIn. Com o ASP.NET Core, você pode gerenciar facilmente os segredos do aplicativo, que são uma maneira de armazenar e usar informações confidenciais sem a necessidade de expô-los no código. 

## <a name="authentication-vs-authorization"></a>Autenticação versus Autorização

A autenticação é um processo em que um usuário fornece credenciais que são comparadas àquelas armazenadas em um sistema operacional, num banco de dados, no aplicativo ou no recurso. Se elas corresponderem, os usuários se autenticarão com êxito e, assim, poderão realizar ações para as quais são autorizados, durante um processo de autorização. A autorização é o processo que determina o que um usuário pode fazer. 

Outra forma de pensar na autenticação é considerá-la como uma maneira de entrar em um espaço, como um servidor, um banco de dados, um aplicativo ou um recurso, ao passo que a autorização refere-se a quais ações o usuário poderá executar em que objetos dentro desse espaço (servidor, banco de dados ou aplicativo).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilidades comuns no software

O ASP.NET Core e o EF contêm recursos que ajudam a proteger seus aplicativos e impedir violações de segurança. A seguinte lista de links leva à documentação com detalhe de técnicas para evitar as vulnerabilidades de segurança mais comuns em aplicativos Web:

* [Ataques de script entre sites](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Ataques de injeção de SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [CSRF (solicitação intersite forjada)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Ataques de redirecionamento aberto](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Há mais vulnerabilidades sobre as quais você deve estar atento. Para obter mais informações, consulte a seção neste documento em *Documentação de segurança do ASP.NET*. 

## <a name="aspnet-security-documentation"></a>Documentação de segurança do ASP.NET

*   [Autenticação](authentication/index.md)
    *   [Introdução ao Identity](authentication/identity.md)
    *   [Habilitar a autenticação usando o Facebook, o Google e outros provedores externos](authentication/social/index.md)
    * [Configurar a Autenticação do Windows](authentication/windowsauth.md)
    *   [Confirmação de conta e recuperação de senha](authentication/accconfirm.md)
    *   [Autenticação de dois fatores com SMS](authentication/2fa.md) 
    *   [Usar a autenticação de cookie sem o Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrar o Azure AD em um aplicativo Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Chamar uma API Web ASP.NET Core em um aplicativo do WPF usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Chamar uma API Web em um aplicativo Web ASP.NET Core usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Um aplicativo Web ASP.NET Core com o Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Proteger aplicativos ASP.NET Core com o IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorização](authorization/index.md)
    *   [Introdução](authorization/introduction.md)
    *   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
    *   [Autorização simples](authorization/simple.md)
    *   [Autorização baseada em função](authorization/roles.md)
    *   [Autorização baseada em declarações](authorization/claims.md)
    *   [Autorização baseada em política](authorization/policies.md)
    *   [Injeção de dependência em manipuladores de requisitos](authorization/dependencyinjection.md)
    *   [Autorização baseada em recursos](authorization/resourcebased.md)
    *   [Autorização baseada em exibição](authorization/views.md)
    *   [Limitar a identidade por esquema](authorization/limitingidentitybyscheme.md)
*   [Proteção de dados](data-protection/index.md)
    *   [Introdução à proteção de dados](data-protection/introduction.md)
    *   [Introdução às APIs de proteção de dados](data-protection/using-data-protection.md)
    *   [APIs de consumidor](data-protection/consumer-apis/index.md)
        *   [Visão geral das APIs de consumidor](data-protection/consumer-apis/overview.md)
        *   [Cadeias de caracteres de finalidade](data-protection/consumer-apis/purpose-strings.md)
        *   [Multilocação e hierarquia de finalidade](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hash de senha](data-protection/consumer-apis/password-hashing.md)
        *   [Limitar o tempo de vida de cargas protegidas](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Desproteger cargas cujas chaves foram revogadas](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Configuração](data-protection/configuration/index.md)
        *   [Configurar a proteção de dados](data-protection/configuration/overview.md)
        *   [Configurações padrão](data-protection/configuration/default-settings.md)
        *   [Política ampla de computador](data-protection/configuration/machine-wide-policy.md)
        *   [Cenários sem reconhecimento de DI](data-protection/configuration/non-di-scenarios.md)
    *   [APIs de extensibilidade](data-protection/extensibility/index.md)
        *   [Extensibilidade da criptografia básica](data-protection/extensibility/core-crypto.md)
        *   [Extensibilidade de gerenciamento de chaves](data-protection/extensibility/key-management.md)
        *   [APIs diversas](data-protection/extensibility/misc-apis.md)
    *   [Implementação](data-protection/implementation/index.md)
        *   [Detalhes de criptografia autenticada](data-protection/implementation/authenticated-encryption-details.md)
        *   [Derivação de subchaves e criptografia autenticada](data-protection/implementation/subkeyderivation.md)
        *   [Cabeçalhos de contexto](data-protection/implementation/context-headers.md)
        *   [Gerenciamento de chaves](data-protection/implementation/key-management.md)
        *   [Provedores de armazenamento de chaves](data-protection/implementation/key-storage-providers.md)
        *   [Criptografia de chave em repouso](data-protection/implementation/key-encryption-at-rest.md)
        *   [Imutabilidade de chave e alteração de configurações](data-protection/implementation/key-immutability.md)
        *   [Formato do armazenamento de chaves](data-protection/implementation/key-storage-format.md)
        *   [Provedores de proteção de dados efêmeros](data-protection/implementation/key-storage-ephemeral.md)
    *   [Compatibilidade](data-protection/compatibility/index.md)
        *   [Compartilhar cookies entre aplicativos](data-protection/compatibility/cookie-sharing.md)
        *   [Substitua <machineKey> no ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
*   [Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento](app-secrets.md)
*   [Provedor de configuração do Azure Key Vault](key-vault-configuration.md)
*   [Impor o SSL](enforcing-ssl.md)
*   [Falsificação anti-solicitação](anti-request-forgery.md)
*   [Prevenir ataques de redirecionamento abertos](preventing-open-redirects.md)
*   [Evitar scripts entre sites](cross-site-scripting.md)
*   [Habilitar o CORS (Solicitações Entre Origens)](cors.md)
