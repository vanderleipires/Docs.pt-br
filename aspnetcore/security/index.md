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
ms.openlocfilehash: e8045b8804d1e7915cd81d697d43a173e33a7119
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="security"></a><span data-ttu-id="1edd9-103">Segurança</span><span class="sxs-lookup"><span data-stu-id="1edd9-103">Security</span></span>

*   [<span data-ttu-id="1edd9-104">Autenticação</span><span class="sxs-lookup"><span data-stu-id="1edd9-104">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="1edd9-105">Introdução ao Identity</span><span class="sxs-lookup"><span data-stu-id="1edd9-105">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="1edd9-106">Habilitando a autenticação usando o Facebook, o Google e outros provedores externos</span><span class="sxs-lookup"><span data-stu-id="1edd9-106">Enabling authentication using Facebook, Google and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="1edd9-107">Configurar a Autenticação do Windows</span><span class="sxs-lookup"><span data-stu-id="1edd9-107">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="1edd9-108">Confirmação de conta e recuperação de senha</span><span class="sxs-lookup"><span data-stu-id="1edd9-108">Account Confirmation and Password Recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="1edd9-109">Autenticação de dois fatores com SMS</span><span class="sxs-lookup"><span data-stu-id="1edd9-109">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="1edd9-110">Usando a autenticação de cookie sem o ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="1edd9-110">Using Cookie Authentication without ASP.NET Core Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="1edd9-111">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1edd9-111">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="1edd9-112">Integrando o Azure AD em um aplicativo Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1edd9-112">Integrating Azure AD Into an ASP.NET Core Web App</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="1edd9-113">Chamando uma API Web ASP.NET Core em um aplicativo do WPF usando o Azure AD</span><span class="sxs-lookup"><span data-stu-id="1edd9-113">Calling a ASP.NET Core Web API From a WPF Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="1edd9-114">Chamando uma API Web em um aplicativo Web ASP.NET Core usando o Azure AD</span><span class="sxs-lookup"><span data-stu-id="1edd9-114">Calling a Web API in an ASP.NET Core Web Application Using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="1edd9-115">Um aplicativo Web ASP.NET Core com o Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="1edd9-115">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="1edd9-116">Protegendo aplicativos ASP.NET Core com o IdentityServer4</span><span class="sxs-lookup"><span data-stu-id="1edd9-116">Securing ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="1edd9-117">Autorização</span><span class="sxs-lookup"><span data-stu-id="1edd9-117">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="1edd9-118">Introdução</span><span class="sxs-lookup"><span data-stu-id="1edd9-118">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="1edd9-119">Criar um aplicativo com os dados do usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="1edd9-119">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="1edd9-120">Autorização simples</span><span class="sxs-lookup"><span data-stu-id="1edd9-120">Simple Authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="1edd9-121">Autorização baseada em função</span><span class="sxs-lookup"><span data-stu-id="1edd9-121">Role based Authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="1edd9-122">Autorização baseada em declarações</span><span class="sxs-lookup"><span data-stu-id="1edd9-122">Claims-Based Authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="1edd9-123">Autorização baseada em política personalizada</span><span class="sxs-lookup"><span data-stu-id="1edd9-123">Custom Policy-Based Authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="1edd9-124">Injeção de dependência em manipuladores de requisitos</span><span class="sxs-lookup"><span data-stu-id="1edd9-124">Dependency Injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="1edd9-125">Autorização baseada em recurso</span><span class="sxs-lookup"><span data-stu-id="1edd9-125">Resource Based Authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="1edd9-126">Autorização baseada em exibição</span><span class="sxs-lookup"><span data-stu-id="1edd9-126">View Based Authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="1edd9-127">Limitando a identidade por esquema</span><span class="sxs-lookup"><span data-stu-id="1edd9-127">Limiting identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="1edd9-128">Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="1edd9-128">Data Protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="1edd9-129">Introdução à Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="1edd9-129">Introduction to Data Protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="1edd9-130">Introdução às APIs de Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="1edd9-130">Getting Started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="1edd9-131">APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="1edd9-131">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="1edd9-132">Visão geral das APIs de consumidor</span><span class="sxs-lookup"><span data-stu-id="1edd9-132">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="1edd9-133">Cadeias de caracteres de finalidade</span><span class="sxs-lookup"><span data-stu-id="1edd9-133">Purpose Strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="1edd9-134">Multilocação e hierarquia de finalidade</span><span class="sxs-lookup"><span data-stu-id="1edd9-134">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="1edd9-135">Hash de senha</span><span class="sxs-lookup"><span data-stu-id="1edd9-135">Password Hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="1edd9-136">Limitando o tempo de vida de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="1edd9-136">Limiting the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="1edd9-137">Desprotegendo cargas cujas chaves foram revogadas</span><span class="sxs-lookup"><span data-stu-id="1edd9-137">Unprotecting payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="1edd9-138">Configuração</span><span class="sxs-lookup"><span data-stu-id="1edd9-138">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="1edd9-139">Configurando a Proteção de Dados</span><span class="sxs-lookup"><span data-stu-id="1edd9-139">Configuring Data Protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="1edd9-140">Configurações padrão</span><span class="sxs-lookup"><span data-stu-id="1edd9-140">Default Settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="1edd9-141">Política para todo o computador</span><span class="sxs-lookup"><span data-stu-id="1edd9-141">Machine Wide Policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="1edd9-142">Cenários sem reconhecimento de DI</span><span class="sxs-lookup"><span data-stu-id="1edd9-142">Non DI Aware Scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="1edd9-143">APIs de extensibilidade</span><span class="sxs-lookup"><span data-stu-id="1edd9-143">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="1edd9-144">Extensibilidade da criptografia básica</span><span class="sxs-lookup"><span data-stu-id="1edd9-144">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="1edd9-145">Extensibilidade de gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="1edd9-145">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="1edd9-146">APIs diversas</span><span class="sxs-lookup"><span data-stu-id="1edd9-146">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="1edd9-147">Implementação</span><span class="sxs-lookup"><span data-stu-id="1edd9-147">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="1edd9-148">Detalhes da criptografia autenticada.</span><span class="sxs-lookup"><span data-stu-id="1edd9-148">Authenticated encryption details.</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="1edd9-149">Derivação de subchaves e criptografia autenticada</span><span class="sxs-lookup"><span data-stu-id="1edd9-149">Subkey Derivation and Authenticated Encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="1edd9-150">Cabeçalhos de contexto</span><span class="sxs-lookup"><span data-stu-id="1edd9-150">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="1edd9-151">Gerenciamento de chaves</span><span class="sxs-lookup"><span data-stu-id="1edd9-151">Key Management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="1edd9-152">Provedores de armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="1edd9-152">Key Storage Providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="1edd9-153">Criptografia de chave em repouso</span><span class="sxs-lookup"><span data-stu-id="1edd9-153">Key Encryption At Rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="1edd9-154">Imutabilidade de chave e alteração de configurações</span><span class="sxs-lookup"><span data-stu-id="1edd9-154">Key Immutability and Changing Settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="1edd9-155">Formato do armazenamento de chaves</span><span class="sxs-lookup"><span data-stu-id="1edd9-155">Key Storage Format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="1edd9-156">Provedores de proteção de dados efêmeros</span><span class="sxs-lookup"><span data-stu-id="1edd9-156">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="1edd9-157">Compatibilidade</span><span class="sxs-lookup"><span data-stu-id="1edd9-157">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="1edd9-158">Compartilhando cookies entre aplicativos</span><span class="sxs-lookup"><span data-stu-id="1edd9-158">Sharing cookies between applications</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="1edd9-159">Substituindo <machineKey> no ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1edd9-159">Replacing <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="1edd9-160">Criar um aplicativo com os dados do usuário protegidos por autorização</span><span class="sxs-lookup"><span data-stu-id="1edd9-160">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="1edd9-161">Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="1edd9-161">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="1edd9-162">Provedor de configuração do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1edd9-162">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="1edd9-163">Impondo o SSL</span><span class="sxs-lookup"><span data-stu-id="1edd9-163">Enforcing SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="1edd9-164">Configurando HTTPS para o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="1edd9-164">Setting up HTTPS for development</span></span>](https.md)
*   [<span data-ttu-id="1edd9-165">Falsificação anti-solicitação</span><span class="sxs-lookup"><span data-stu-id="1edd9-165">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="1edd9-166">Prevenindo ataques de redirecionamento abertos</span><span class="sxs-lookup"><span data-stu-id="1edd9-166">Preventing Open Redirect Attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="1edd9-167">Prevenindo scripts entre sites</span><span class="sxs-lookup"><span data-stu-id="1edd9-167">Preventing Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="1edd9-168">Habilitando o CORS (Solicitações Entre Origens)</span><span class="sxs-lookup"><span data-stu-id="1edd9-168">Enabling Cross-Origin Requests (CORS)</span></span>](cors.md)
