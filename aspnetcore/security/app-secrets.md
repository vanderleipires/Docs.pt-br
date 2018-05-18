---
title: Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET
author: rick-anderson
description: Saiba como armazenar e recuperar informações confidenciais, como segredos do aplicativo durante o desenvolvimento de um aplicativo ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="5cdfe-103">Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5cdfe-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="5cdfe-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5cdfe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cdfe-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="5cdfe-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5cdfe-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="5cdfe-107">Este documento explica técnicas para armazenar e recuperar dados confidenciais durante o desenvolvimento de um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="5cdfe-108">Você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar segredos de produção em desenvolvimento ou modo de teste.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="5cdfe-109">Você pode armazenar e proteger segredos de teste e produção do Azure com o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="5cdfe-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="5cdfe-110">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="5cdfe-110">Environment variables</span></span>

<span data-ttu-id="5cdfe-111">Variáveis de ambiente são usadas para evitar o armazenamento de segredos do aplicativo no código ou em arquivos de configuração local.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="5cdfe-112">Variáveis de ambiente substituem os valores de configuração para todas as fontes de configuração especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-113">Configurar a leitura dos valores de variável de ambiente chamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="5cdfe-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="5cdfe-115">Considere um aplicativo da web ASP.NET Core no qual **contas de usuário individuais** segurança está habilitada.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="5cdfe-116">Uma cadeia de caracteres de conexão de banco de dados padrão está incluída no projeto de *appSettings. JSON* arquivo com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="5cdfe-117">A cadeia de caracteres de conexão padrão é para o LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="5cdfe-118">Durante a implantação do aplicativo, o `DefaultConnection` valor chave pode ser substituído pelo valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="5cdfe-119">A variável de ambiente pode armazenar a cadeia de caracteres de conexão completa com credenciais confidenciais.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="5cdfe-120">Variáveis de ambiente são geralmente armazenadas em texto sem formatação e não criptografado.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="5cdfe-121">Se o computador ou o processo estiver comprometido, as variáveis de ambiente podem ser acessadas por indivíduos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="5cdfe-122">Medidas adicionais para evitar a divulgação de segredos do usuário podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="5cdfe-123">Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="5cdfe-123">Secret Manager</span></span>

<span data-ttu-id="5cdfe-124">A ferramenta Gerenciador de segredo armazena dados confidenciais durante o desenvolvimento de um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="5cdfe-125">Nesse contexto, uma parte dos dados confidenciais é um segredo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="5cdfe-126">Segredos do aplicativo são armazenados em um local separado da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="5cdfe-127">Os segredos do aplicativo são compartilhados entre vários projetos ou associados a um projeto específico.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="5cdfe-128">Os segredos do aplicativo não são verificados no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="5cdfe-129">A ferramenta Gerenciador de segredo não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="5cdfe-130">Ele destina-se apenas para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-130">It's for development purposes only.</span></span> <span data-ttu-id="5cdfe-131">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="5cdfe-132">Como funciona a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="5cdfe-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="5cdfe-133">A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="5cdfe-134">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="5cdfe-135">Os valores são armazenados em um [JSON](https://json.org/) arquivo de configuração em uma pasta de perfil de usuário protegido pelo sistema no computador local:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="5cdfe-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="5cdfe-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="5cdfe-137">& MacOS do Linux: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="5cdfe-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="5cdfe-138">Na anterior caminhos de arquivo, substitua `<user_secrets_id>` com o `UserSecretsId` valor especificado no *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="5cdfe-139">Não grave o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="5cdfe-140">Esses detalhes de implementação podem ser alterado.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-140">These implementation details may change.</span></span> <span data-ttu-id="5cdfe-141">Por exemplo, os valores de segredo não estão criptografados, mas podem ser no futuro.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="5cdfe-142">Instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="5cdfe-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="5cdfe-143">A ferramenta Gerenciador de segredo é fornecida com a CLI do núcleo do .NET no .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="5cdfe-144">Para .NET Core SDK 2.0 e versões anteriores, a instalação da ferramenta é necessária.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="5cdfe-145">Instalar o [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacote NuGet em seu projeto do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="5cdfe-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="5cdfe-147">Execute o seguinte comando em um shell de comando para validar a instalação da ferramenta:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="5cdfe-148">A ferramenta Gerenciador de segredo exibe a Ajuda do comando de exemplo de uso e opções:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="5cdfe-149">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="5cdfe-150">Definir um segredo</span><span class="sxs-lookup"><span data-stu-id="5cdfe-150">Set a secret</span></span>

<span data-ttu-id="5cdfe-151">A ferramenta Gerenciador de segredo opera em específica do projeto configurações armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="5cdfe-152">Para usar os segredos do usuário, defina um `UserSecretsId` elemento dentro de um `PropertyGroup` do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="5cdfe-153">O valor de `UserSecretsId` é arbitrária, mas é exclusiva ao projeto.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="5cdfe-154">Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="5cdfe-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="5cdfe-157">No Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="5cdfe-158">Esse gesto adiciona um `UserSecretsId` elemento, preenchido com um GUID para o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="5cdfe-159">O Visual Studio abrirá um *secrets.json* arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="5cdfe-160">Substitua o conteúdo do *secrets.json* com os pares chave-valor a ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="5cdfe-161">Por exemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="5cdfe-162">Defina um segredo do aplicativo consiste em uma chave e seu valor.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="5cdfe-163">O segredo é associado ao projeto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="5cdfe-164">Por exemplo, execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="5cdfe-165">No exemplo anterior, os dois-pontos indica que `Movies` é um objeto literal com um `ServiceApiKey` propriedade.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="5cdfe-166">A ferramenta Gerenciador de segredo pode ser usada de outros diretórios muito.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="5cdfe-167">Use o `--project` opção de fornecer o caminho do sistema de arquivos no qual o *. csproj* arquivo existe.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="5cdfe-168">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="5cdfe-169">Definir vários segredos</span><span class="sxs-lookup"><span data-stu-id="5cdfe-169">Set multiple secrets</span></span>

<span data-ttu-id="5cdfe-170">Um lote de segredos pode ser definido ao canalizar JSON para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="5cdfe-171">No exemplo a seguir, o *input.json* conteúdo do arquivo é direcionado para o `set` comando no Windows:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="5cdfe-172">Use o seguinte comando em macOS e Linux:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="5cdfe-173">Acessar um segredo</span><span class="sxs-lookup"><span data-stu-id="5cdfe-173">Access a secret</span></span>

<span data-ttu-id="5cdfe-174">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso para segredos do Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="5cdfe-175">Se direcionando o .NET Core 1. x ou do .NET Framework, instale o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-176">Adicione a fonte de configuração de segredos do usuário para o `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="5cdfe-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="5cdfe-178">Segredos do usuário podem ser recuperados por meio de `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="5cdfe-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="5cdfe-181">Substituição de cadeia de caracteres com segredos</span><span class="sxs-lookup"><span data-stu-id="5cdfe-181">String replacement with secrets</span></span>

<span data-ttu-id="5cdfe-182">Armazenar senhas em texto sem formatação é arriscado.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="5cdfe-183">Por exemplo, uma cadeia de caracteres de conexão do banco de dados armazenado em *appSettings. JSON* pode incluir uma senha para o usuário especificado:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="5cdfe-184">Uma abordagem mais segura é armazenar a senha como um segredo.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="5cdfe-185">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="5cdfe-186">Substitua a senha em *appSettings. JSON* com um espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="5cdfe-187">No exemplo a seguir, `{0}` é usado como o espaço reservado para formar um [cadeia de caracteres de formato composto](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="5cdfe-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="5cdfe-188">Valor do segredo pode ser inserida no espaço reservado para concluir a cadeia de caracteres de conexão:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="5cdfe-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="5cdfe-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="5cdfe-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="5cdfe-191">Lista os segredos</span><span class="sxs-lookup"><span data-stu-id="5cdfe-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5cdfe-192">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="5cdfe-193">A seguinte saída é exibida:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="5cdfe-194">No exemplo anterior, dois-pontos em nomes de chave indica que a hierarquia de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="5cdfe-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="5cdfe-195">Remover um segredo único</span><span class="sxs-lookup"><span data-stu-id="5cdfe-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5cdfe-196">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="5cdfe-197">O aplicativo *secrets.json* arquivo foi modificado para remover o par chave-valor associado a `MoviesConnectionString` chave:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="5cdfe-198">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="5cdfe-199">Remover todos os segredos</span><span class="sxs-lookup"><span data-stu-id="5cdfe-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5cdfe-200">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="5cdfe-201">Todos os segredos do usuário para o aplicativo tem sido excluídos do *secrets.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="5cdfe-202">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="5cdfe-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="5cdfe-203">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="5cdfe-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>