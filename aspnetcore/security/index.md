---
title: "Segurança"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: aa22d072a6ef0ff105d67c2bfc5c335511d6c0cd
ms.sourcegitcommit: aa6951e0c2e62209bf7c25e3b3138f04eb92898d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2017
---
# <a name="security"></a>Segurança

*   [Autenticação](authentication/index.md)
    *   [Introdução ao Identity](authentication/identity.md)
    *   [Habilitando a autenticação usando o Facebook, o Google e outros provedores externos](authentication/social/index.md)
    * [Configurar a Autenticação do Windows](authentication/windowsauth.md)
    *   [Confirmação de conta e recuperação de senha](authentication/accconfirm.md)
    *   [Autenticação de dois fatores com SMS](authentication/2fa.md) 
    *   [Usando a autenticação de cookie sem o ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrando o Azure AD em um aplicativo Web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore)
        *   [Chamando uma API Web ASP.NET Core em um aplicativo do WPF usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore)
        *   [Chamando uma API Web em um aplicativo Web ASP.NET Core usando o Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Um aplicativo Web ASP.NET Core com o Azure AD B2C](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c/)
    *   [Protegendo aplicativos ASP.NET Core com o IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorização](authorization/index.md)
    *   [Introdução](authorization/introduction.md)
    *   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
    *   [Autorização simples](authorization/simple.md)
    *   [Autorização baseada em função](authorization/roles.md)
    *   [Autorização baseada em declarações](authorization/claims.md)
    *   [Autorização baseada em política personalizada](authorization/policies.md)
    *   [Injeção de dependência em manipuladores de requisitos](authorization/dependencyinjection.md)
    *   [Autorização baseada em recurso](authorization/resourcebased.md)
    *   [Autorização baseada em exibição](authorization/views.md)
    *   [Limitando a identidade por esquema](authorization/limitingidentitybyscheme.md)
*   [Proteção de Dados](data-protection/index.md)
    *   [Introdução à Proteção de Dados](data-protection/introduction.md)
    *   [Introdução às APIs de Proteção de Dados](data-protection/using-data-protection.md)
    *   [APIs de consumidor](data-protection/consumer-apis/index.md)
        *   [Visão geral das APIs de consumidor](data-protection/consumer-apis/overview.md)
        *   [Cadeias de caracteres de finalidade](data-protection/consumer-apis/purpose-strings.md)
        *   [Multilocação e hierarquia de finalidade](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hash de senha](data-protection/consumer-apis/password-hashing.md)
        *   [Limitando o tempo de vida de cargas protegidas](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Desprotegendo cargas cujas chaves foram revogadas](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Configuração](data-protection/configuration/index.md)
        *   [Configurando a Proteção de Dados](data-protection/configuration/overview.md)
        *   [Configurações padrão](data-protection/configuration/default-settings.md)
        *   [Política para todo o computador](data-protection/configuration/machine-wide-policy.md)
        *   [Cenários sem reconhecimento de DI](data-protection/configuration/non-di-scenarios.md)
    *   [APIs de extensibilidade](data-protection/extensibility/index.md)
        *   [Extensibilidade da criptografia básica](data-protection/extensibility/core-crypto.md)
        *   [Extensibilidade de gerenciamento de chaves](data-protection/extensibility/key-management.md)
        *   [APIs diversas](data-protection/extensibility/misc-apis.md)
    *   [Implementação](data-protection/implementation/index.md)
        *   [Detalhes da criptografia autenticada.](data-protection/implementation/authenticated-encryption-details.md)
        *   [Derivação de subchaves e criptografia autenticada](data-protection/implementation/subkeyderivation.md)
        *   [Cabeçalhos de contexto](data-protection/implementation/context-headers.md)
        *   [Gerenciamento de chaves](data-protection/implementation/key-management.md)
        *   [Provedores de armazenamento de chaves](data-protection/implementation/key-storage-providers.md)
        *   [Criptografia de chave em repouso](data-protection/implementation/key-encryption-at-rest.md)
        *   [Imutabilidade de chave e alteração de configurações](data-protection/implementation/key-immutability.md)
        *   [Formato do armazenamento de chaves](data-protection/implementation/key-storage-format.md)
        *   [Provedores de proteção de dados efêmeros](data-protection/implementation/key-storage-ephemeral.md)
    *   [Compatibilidade](data-protection/compatibility/index.md)
        *   [Compartilhando cookies entre aplicativos](data-protection/compatibility/cookie-sharing.md)
        *   [Substituindo <machineKey> no ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Criar um aplicativo com os dados do usuário protegidos por autorização](xref:security/authorization/secure-data)
*   [Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento](app-secrets.md)
*   [Provedor de configuração do Azure Key Vault](key-vault-configuration.md)
*   [Impondo o SSL](enforcing-ssl.md)
*   [Configurando HTTPS para o desenvolvimento](https.md)
*   [Falsificação anti-solicitação](anti-request-forgery.md)
*   [Prevenindo ataques de redirecionamento abertos](preventing-open-redirects.md)
*   [Prevenindo scripts entre sites](cross-site-scripting.md)
*   [Habilitando o CORS (Solicitações Entre Origens)](cors.md)
