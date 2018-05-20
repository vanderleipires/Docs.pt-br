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
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="df3ab-103">Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="df3ab-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="df3ab-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="df3ab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df3ab-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="df3ab-106">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df3ab-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="df3ab-107">Este documento explica técnicas para armazenar e recuperar dados confidenciais durante o desenvolvimento de um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df3ab-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="df3ab-108">Você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar segredos de produção em desenvolvimento ou modo de teste.</span><span class="sxs-lookup"><span data-stu-id="df3ab-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="df3ab-109">Você pode armazenar e proteger segredos de teste e produção do Azure com o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="df3ab-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="df3ab-110">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="df3ab-110">Environment variables</span></span>

<span data-ttu-id="df3ab-111">Variáveis de ambiente são usadas para evitar o armazenamento de segredos do aplicativo no código ou em arquivos de configuração local.</span><span class="sxs-lookup"><span data-stu-id="df3ab-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="df3ab-112">Variáveis de ambiente substituem os valores de configuração para todas as fontes de configuração especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="df3ab-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-113">Configurar a leitura dos valores de variável de ambiente chamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="df3ab-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="df3ab-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="df3ab-115">Considere um aplicativo da web ASP.NET Core no qual **contas de usuário individuais** segurança está habilitada.</span><span class="sxs-lookup"><span data-stu-id="df3ab-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="df3ab-116">Uma cadeia de caracteres de conexão de banco de dados padrão está incluída no projeto de *appSettings. JSON* arquivo com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="df3ab-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="df3ab-117">A cadeia de caracteres de conexão padrão é para o LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="df3ab-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="df3ab-118">Durante a implantação do aplicativo, o `DefaultConnection` valor chave pode ser substituído pelo valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="df3ab-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="df3ab-119">A variável de ambiente pode armazenar a cadeia de caracteres de conexão completa com credenciais confidenciais.</span><span class="sxs-lookup"><span data-stu-id="df3ab-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="df3ab-120">Variáveis de ambiente são geralmente armazenadas em texto sem formatação e não criptografado.</span><span class="sxs-lookup"><span data-stu-id="df3ab-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="df3ab-121">Se o computador ou o processo estiver comprometido, as variáveis de ambiente podem ser acessadas por indivíduos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="df3ab-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="df3ab-122">Medidas adicionais para evitar a divulgação de segredos do usuário podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="df3ab-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="df3ab-123">Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="df3ab-123">Secret Manager</span></span>

<span data-ttu-id="df3ab-124">A ferramenta Gerenciador de segredo armazena dados confidenciais durante o desenvolvimento de um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df3ab-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="df3ab-125">Nesse contexto, uma parte dos dados confidenciais é um segredo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="df3ab-126">Segredos do aplicativo são armazenados em um local separado da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="df3ab-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="df3ab-127">Os segredos do aplicativo são compartilhados entre vários projetos ou associados a um projeto específico.</span><span class="sxs-lookup"><span data-stu-id="df3ab-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="df3ab-128">Os segredos do aplicativo não são verificados no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="df3ab-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="df3ab-129">A ferramenta Gerenciador de segredo não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="df3ab-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="df3ab-130">Ele destina-se apenas para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="df3ab-130">It's for development purposes only.</span></span> <span data-ttu-id="df3ab-131">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="df3ab-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="df3ab-132">Como funciona a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="df3ab-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="df3ab-133">A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="df3ab-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="df3ab-134">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="df3ab-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="df3ab-135">Os valores são armazenados em um [JSON](https://json.org/) arquivo de configuração em uma pasta de perfil de usuário protegido pelo sistema no computador local:</span><span class="sxs-lookup"><span data-stu-id="df3ab-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="df3ab-136">Windows</span><span class="sxs-lookup"><span data-stu-id="df3ab-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="df3ab-137">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="df3ab-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="df3ab-138">macOS</span><span class="sxs-lookup"><span data-stu-id="df3ab-138">macOS</span></span>](#tab/macos)

<span data-ttu-id="df3ab-139">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="df3ab-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="df3ab-140">Linux</span><span class="sxs-lookup"><span data-stu-id="df3ab-140">Linux</span></span>](#tab/linux)

<span data-ttu-id="df3ab-141">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="df3ab-141">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="df3ab-142">Na anterior caminhos de arquivo, substitua `<user_secrets_id>` com o `UserSecretsId` valor especificado no *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-142">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="df3ab-143">Não grave o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-143">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="df3ab-144">Esses detalhes de implementação podem ser alterado.</span><span class="sxs-lookup"><span data-stu-id="df3ab-144">These implementation details may change.</span></span> <span data-ttu-id="df3ab-145">Por exemplo, os valores de segredo não estão criptografados, mas podem ser no futuro.</span><span class="sxs-lookup"><span data-stu-id="df3ab-145">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="df3ab-146">Instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="df3ab-146">Install the Secret Manager tool</span></span>

<span data-ttu-id="df3ab-147">A ferramenta Gerenciador de segredo é fornecida com a CLI do núcleo do .NET no .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="df3ab-147">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="df3ab-148">Para .NET Core SDK 2.0 e versões anteriores, a instalação da ferramenta é necessária.</span><span class="sxs-lookup"><span data-stu-id="df3ab-148">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="df3ab-149">Instalar o [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacote NuGet em seu projeto do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="df3ab-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="df3ab-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="df3ab-151">Execute o seguinte comando em um shell de comando para validar a instalação da ferramenta:</span><span class="sxs-lookup"><span data-stu-id="df3ab-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="df3ab-152">A ferramenta Gerenciador de segredo exibe a Ajuda do comando de exemplo de uso e opções:</span><span class="sxs-lookup"><span data-stu-id="df3ab-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="df3ab-153">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="df3ab-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="df3ab-154">Definir um segredo</span><span class="sxs-lookup"><span data-stu-id="df3ab-154">Set a secret</span></span>

<span data-ttu-id="df3ab-155">A ferramenta Gerenciador de segredo opera em específica do projeto configurações armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="df3ab-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="df3ab-156">Para usar os segredos do usuário, defina um `UserSecretsId` elemento dentro de um `PropertyGroup` do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="df3ab-157">O valor de `UserSecretsId` é arbitrária, mas é exclusiva ao projeto.</span><span class="sxs-lookup"><span data-stu-id="df3ab-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="df3ab-158">Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="df3ab-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="df3ab-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="df3ab-161">No Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="df3ab-161">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="df3ab-162">Esse gesto adiciona um `UserSecretsId` elemento, preenchido com um GUID para o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-162">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="df3ab-163">O Visual Studio abrirá um *secrets.json* arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="df3ab-163">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="df3ab-164">Substitua o conteúdo do *secrets.json* com os pares chave-valor a ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="df3ab-164">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="df3ab-165">Por exemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-165">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="df3ab-166">Defina um segredo do aplicativo consiste em uma chave e seu valor.</span><span class="sxs-lookup"><span data-stu-id="df3ab-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="df3ab-167">O segredo é associado ao projeto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="df3ab-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="df3ab-168">Por exemplo, execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="df3ab-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="df3ab-169">No exemplo anterior, os dois-pontos indica que `Movies` é um objeto literal com um `ServiceApiKey` propriedade.</span><span class="sxs-lookup"><span data-stu-id="df3ab-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="df3ab-170">A ferramenta Gerenciador de segredo pode ser usada de outros diretórios muito.</span><span class="sxs-lookup"><span data-stu-id="df3ab-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="df3ab-171">Use o `--project` opção de fornecer o caminho do sistema de arquivos no qual o *. csproj* arquivo existe.</span><span class="sxs-lookup"><span data-stu-id="df3ab-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="df3ab-172">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="df3ab-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="df3ab-173">Definir vários segredos</span><span class="sxs-lookup"><span data-stu-id="df3ab-173">Set multiple secrets</span></span>

<span data-ttu-id="df3ab-174">Um lote de segredos pode ser definido ao canalizar JSON para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="df3ab-174">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="df3ab-175">No exemplo a seguir, o *input.json* conteúdo do arquivo é direcionado para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="df3ab-175">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="df3ab-176">Windows</span><span class="sxs-lookup"><span data-stu-id="df3ab-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="df3ab-177">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="df3ab-177">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="df3ab-178">macOS</span><span class="sxs-lookup"><span data-stu-id="df3ab-178">macOS</span></span>](#tab/macos)

<span data-ttu-id="df3ab-179">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="df3ab-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="df3ab-180">Linux</span><span class="sxs-lookup"><span data-stu-id="df3ab-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="df3ab-181">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="df3ab-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="df3ab-182">Acessar um segredo</span><span class="sxs-lookup"><span data-stu-id="df3ab-182">Access a secret</span></span>

<span data-ttu-id="df3ab-183">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso para segredos do Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-183">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="df3ab-184">Se direcionando o .NET Core 1. x ou do .NET Framework, instale o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="df3ab-184">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-185">Adicione a fonte de configuração de segredos do usuário para o `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="df3ab-185">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="df3ab-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="df3ab-187">Segredos do usuário podem ser recuperados por meio de `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="df3ab-187">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="df3ab-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="df3ab-190">Substituição de cadeia de caracteres com segredos</span><span class="sxs-lookup"><span data-stu-id="df3ab-190">String replacement with secrets</span></span>

<span data-ttu-id="df3ab-191">Armazenar senhas em texto sem formatação é arriscado.</span><span class="sxs-lookup"><span data-stu-id="df3ab-191">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="df3ab-192">Por exemplo, uma cadeia de caracteres de conexão do banco de dados armazenado em *appSettings. JSON* pode incluir uma senha para o usuário especificado:</span><span class="sxs-lookup"><span data-stu-id="df3ab-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="df3ab-193">Uma abordagem mais segura é armazenar a senha como um segredo.</span><span class="sxs-lookup"><span data-stu-id="df3ab-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="df3ab-194">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="df3ab-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="df3ab-195">Substitua a senha em *appSettings. JSON* com um espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="df3ab-195">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="df3ab-196">No exemplo a seguir, `{0}` é usado como o espaço reservado para formar um [cadeia de caracteres de formato composto](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="df3ab-196">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="df3ab-197">Valor do segredo pode ser inserida no espaço reservado para concluir a cadeia de caracteres de conexão:</span><span class="sxs-lookup"><span data-stu-id="df3ab-197">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="df3ab-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="df3ab-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="df3ab-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="df3ab-200">Lista os segredos</span><span class="sxs-lookup"><span data-stu-id="df3ab-200">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="df3ab-201">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="df3ab-201">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="df3ab-202">A seguinte saída é exibida:</span><span class="sxs-lookup"><span data-stu-id="df3ab-202">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="df3ab-203">No exemplo anterior, dois-pontos em nomes de chave indica que a hierarquia de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="df3ab-203">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="df3ab-204">Remover um segredo único</span><span class="sxs-lookup"><span data-stu-id="df3ab-204">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="df3ab-205">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="df3ab-205">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="df3ab-206">O aplicativo *secrets.json* arquivo foi modificado para remover o par chave-valor associado a `MoviesConnectionString` chave:</span><span class="sxs-lookup"><span data-stu-id="df3ab-206">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="df3ab-207">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="df3ab-207">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="df3ab-208">Remover todos os segredos</span><span class="sxs-lookup"><span data-stu-id="df3ab-208">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="df3ab-209">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="df3ab-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="df3ab-210">Todos os segredos do usuário para o aplicativo tem sido excluídos do *secrets.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="df3ab-210">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="df3ab-211">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="df3ab-211">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="df3ab-212">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="df3ab-212">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>