---
title: Visão geral sobre a segurança do ASP.NET Core
author: tdykstra
description: Saiba mais sobre conceitos básicos de autenticação, autorização e segurança no ASP.NET Core.
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: 3a1c1ea1ad28fccbe5ae91b0be193938b095f60b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41746066"
---
# <a name="overview-of-aspnet-core-security"></a>Visão geral sobre a segurança do ASP.NET Core

O ASP.NET Core permite que desenvolvedores configurem e gerenciem facilmente a segurança de seus aplicativos. O ASP.NET Core contém recursos para gerenciamento de autenticação, autorização, proteção de dados, imposição de SSL, segredos de aplicativo, proteção contra falsificação de solicitação e gerenciamento de CORS. Esses recursos de segurança permitem que você crie aplicativos de ASP.NET Core robustos e seguros ao mesmo tempo.

## <a name="aspnet-core-security-features"></a>Recursos de segurança do ASP.NET Core

O ASP.NET Core fornece várias ferramentas e bibliotecas para proteger seus aplicativos, incluindo provedores de identidade internos, mas você pode usar serviços de identidade de terceiros, como Facebook, Twitter e LinkedIn. Com o ASP.NET Core, você pode gerenciar facilmente os segredos do aplicativo, que são uma maneira de armazenar e usar informações confidenciais sem a necessidade de expô-los no código.

## <a name="authentication-vs-authorization"></a>Autenticação versus Autorização

A autenticação é um processo em que um usuário fornece credenciais que são comparadas àquelas armazenadas em um sistema operacional, num banco de dados, no aplicativo ou no recurso. Se elas corresponderem, os usuários se autenticarão com êxito e, assim, poderão realizar ações para as quais são autorizados, durante um processo de autorização. A autorização é o processo que determina o que um usuário pode fazer.

Outra forma de pensar na autenticação é considerá-la como uma maneira de entrar em um espaço, como um servidor, um banco de dados, um aplicativo ou um recurso, ao passo que a autorização refere-se a quais ações o usuário poderá executar em que objetos dentro desse espaço (servidor, banco de dados ou aplicativo).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilidades comuns no software

O ASP.NET Core e o EF contêm recursos que ajudam a proteger seus aplicativos e impedir violações de segurança. A seguinte lista de links leva à documentação com detalhe de técnicas para evitar as vulnerabilidades de segurança mais comuns em aplicativos Web:

* [Ataques de script entre sites](xref:security/cross-site-scripting)
* [Ataques de injeção de SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [CSRF (solicitação intersite forjada)](xref:security/anti-request-forgery)
* [Ataques de redirecionamento aberto](xref:security/preventing-open-redirects)

Há mais vulnerabilidades sobre as quais você deve estar atento. Para obter mais informações, consulte a seção neste documento em *Documentação de segurança do ASP.NET Core*.

## <a name="aspnet-core-security-documentation"></a>Documentação de segurança do ASP.NET Core

*   [Autenticação](xref:security/authentication/index)
    *   [Introdução ao Identity](xref:security/authentication/identity)
    *   [Habilitar a autenticação usando o Facebook, o Google e outros provedores externos](xref:security/authentication/social/index)
    *   [Habilitar a autenticação com o Web Services Federation](xref:security/authentication/ws-federation)
    * [Configurar a Autenticação do Windows](xref:security/authentication/windowsauth)
    *   [Confirmação de conta e recuperação de senha](xref:security/authentication/accconfirm)
    *   [Autenticação de dois fatores com SMS](xref:security/authentication/2fa)
    *   [Usar a autenticação de cookie sem o Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrar o Azure AD em um aplicativo Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Chamar uma API Web ASP.NET Core em um aplicativo do WPF usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Chamar uma API Web em um aplicativo Web ASP.NET Core usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Um aplicativo Web ASP.NET Core com o Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Proteger aplicativos ASP.NET Core com o IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorização](xref:security/authorization/index)
    *   [Introdução](xref:security/authorization/introduction)
    *   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
    *   [Autorização simples](xref:security/authorization/simple)
    *   [Autorização baseada em função](xref:security/authorization/roles)
    *   [Autorização baseada em declarações](xref:security/authorization/claims)
    *   [Autorização baseada em política](xref:security/authorization/policies)
    *   [Injeção de dependência em manipuladores de requisitos](xref:security/authorization/dependencyinjection)
    *   [Autorização baseada em recursos](xref:security/authorization/resourcebased)
    *   [Autorização baseada em exibição](xref:security/authorization/views)
    *   [Limitar a identidade por esquema](xref:security/authorization/limitingidentitybyscheme)
*   [Proteção de dados](xref:security/data-protection/index)
    *   [Introdução à proteção de dados](xref:security/data-protection/introduction)
    *   [Introdução às APIs de proteção de dados](xref:security/data-protection/using-data-protection)
    *   [APIs de consumidor](xref:security/data-protection/consumer-apis/index)
        *   [Visão geral das APIs de consumidor](xref:security/data-protection/consumer-apis/overview)
        *   [Cadeias de caracteres de finalidade](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Multilocação e hierarquia de finalidade](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Senhas hash](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Limitar o tempo de vida de cargas protegidas](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Desproteger cargas cujas chaves foram revogadas](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Configuração](xref:security/data-protection/configuration/index)
        *   [Configurar a proteção de dados](xref:security/data-protection/configuration/overview)
        *   [Configurações padrão](xref:security/data-protection/configuration/default-settings)
        *   [Política ampla de computador](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Cenários sem reconhecimento de DI](xref:security/data-protection/configuration/non-di-scenarios)
    *   [APIs de extensibilidade](xref:security/data-protection/extensibility/index)
        *   [Extensibilidade da criptografia básica](xref:security/data-protection/extensibility/core-crypto)
        *   [Extensibilidade de gerenciamento de chaves](xref:security/data-protection/extensibility/key-management)
        *   [APIs diversas](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementação](xref:security/data-protection/implementation/index)
        *   [Detalhes de criptografia autenticada](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Derivação de subchaves e criptografia autenticada](xref:security/data-protection/implementation/subkeyderivation)
        *   [Cabeçalhos de contexto](xref:security/data-protection/implementation/context-headers)
        *   [Gerenciamento de chaves](xref:security/data-protection/implementation/key-management)
        *   [Provedores de armazenamento de chaves](xref:security/data-protection/implementation/key-storage-providers)
        *   [Criptografia de chave em repouso](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Imutabilidade de chave e configurações](xref:security/data-protection/implementation/key-immutability)
        *   [Formato do armazenamento de chaves](xref:security/data-protection/implementation/key-storage-format)
        *   [Provedores de proteção de dados efêmeros](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Compatibilidade](xref:security/data-protection/compatibility/index)
        *   [Substitua <machineKey> no ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
*   [Armazenamento seguro dos segredos do aplicativo no desenvolvimento](xref:security/app-secrets)
*   [Provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration)
*   [Impor o SSL](xref:security/enforcing-ssl)
*   [Falsificação anti-solicitação](xref:security/anti-request-forgery)
*   [Prevenir ataques de redirecionamento abertos](xref:security/preventing-open-redirects)
*   [Evitar scripts entre sites](xref:security/cross-site-scripting)
*   [Habilitar o CORS (Solicitações Entre Origens)](xref:security/cors)
*   [Compartilhar cookies entre aplicativos](xref:security/cookie-sharing)
