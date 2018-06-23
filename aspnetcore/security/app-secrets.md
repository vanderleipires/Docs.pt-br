---
title: Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET
author: rick-anderson
description: Saiba como armazenar e recuperar informações confidenciais, como segredos do aplicativo durante o desenvolvimento de um aplicativo ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314169"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="96276-103">Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="96276-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="96276-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="96276-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="96276-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96276-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="96276-106">Este documento explica técnicas para armazenar e recuperar dados confidenciais durante o desenvolvimento de um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96276-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="96276-107">Você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar segredos de produção em desenvolvimento ou modo de teste.</span><span class="sxs-lookup"><span data-stu-id="96276-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="96276-108">Você pode armazenar e proteger segredos de teste e produção do Azure com o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="96276-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="96276-109">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="96276-109">Environment variables</span></span>

<span data-ttu-id="96276-110">Variáveis de ambiente são usadas para evitar o armazenamento de segredos do aplicativo no código ou em arquivos de configuração local.</span><span class="sxs-lookup"><span data-stu-id="96276-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="96276-111">Variáveis de ambiente substituem os valores de configuração para todas as fontes de configuração especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="96276-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="96276-112">Configurar a leitura dos valores de variável de ambiente chamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="96276-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="96276-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="96276-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="96276-114">Considere um aplicativo da web ASP.NET Core no qual **contas de usuário individuais** segurança está habilitada.</span><span class="sxs-lookup"><span data-stu-id="96276-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="96276-115">Uma cadeia de caracteres de conexão de banco de dados padrão está incluída no projeto de *appSettings. JSON* arquivo com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="96276-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="96276-116">A cadeia de caracteres de conexão padrão é para o LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="96276-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="96276-117">Durante a implantação do aplicativo, o `DefaultConnection` valor chave pode ser substituído pelo valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="96276-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="96276-118">A variável de ambiente pode armazenar a cadeia de caracteres de conexão completa com credenciais confidenciais.</span><span class="sxs-lookup"><span data-stu-id="96276-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="96276-119">Variáveis de ambiente são geralmente armazenadas em texto sem formatação e não criptografado.</span><span class="sxs-lookup"><span data-stu-id="96276-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="96276-120">Se o computador ou o processo estiver comprometido, as variáveis de ambiente podem ser acessadas por indivíduos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="96276-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="96276-121">Medidas adicionais para evitar a divulgação de segredos do usuário podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="96276-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="96276-122">Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="96276-122">Secret Manager</span></span>

<span data-ttu-id="96276-123">A ferramenta Gerenciador de segredo armazena dados confidenciais durante o desenvolvimento de um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96276-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="96276-124">Nesse contexto, uma parte dos dados confidenciais é um segredo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="96276-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="96276-125">Segredos do aplicativo são armazenados em um local separado da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="96276-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="96276-126">Os segredos do aplicativo são compartilhados entre vários projetos ou associados a um projeto específico.</span><span class="sxs-lookup"><span data-stu-id="96276-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="96276-127">Os segredos do aplicativo não são verificados no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="96276-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="96276-128">A ferramenta Gerenciador de segredo não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="96276-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="96276-129">Ele destina-se apenas para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="96276-129">It's for development purposes only.</span></span> <span data-ttu-id="96276-130">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="96276-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="96276-131">Como funciona a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="96276-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="96276-132">A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="96276-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="96276-133">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="96276-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="96276-134">Os valores são armazenados em um arquivo de configuração de JSON em uma pasta de perfil de usuário protegido pelo sistema no computador local:</span><span class="sxs-lookup"><span data-stu-id="96276-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="96276-135">Windows</span><span class="sxs-lookup"><span data-stu-id="96276-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="96276-136">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="96276-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="96276-137">macOS</span><span class="sxs-lookup"><span data-stu-id="96276-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="96276-138">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="96276-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="96276-139">Linux</span><span class="sxs-lookup"><span data-stu-id="96276-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="96276-140">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="96276-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="96276-141">Na anterior caminhos de arquivo, substitua `<user_secrets_id>` com o `UserSecretsId` valor especificado no *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="96276-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="96276-142">Não grave o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="96276-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="96276-143">Esses detalhes de implementação podem ser alterado.</span><span class="sxs-lookup"><span data-stu-id="96276-143">These implementation details may change.</span></span> <span data-ttu-id="96276-144">Por exemplo, os valores de segredo não estão criptografados, mas podem ser no futuro.</span><span class="sxs-lookup"><span data-stu-id="96276-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="96276-145">Instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="96276-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="96276-146">A ferramenta Gerenciador de segredo é fornecida com a CLI do núcleo do .NET a partir do SDK do .NET Core 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="96276-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="96276-147">Para versões do SDK do .NET Core antes 2.1.300, a instalação da ferramenta é necessária.</span><span class="sxs-lookup"><span data-stu-id="96276-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="96276-148">Executar `dotnet --version` de um shell de comando para ver o número de versão do SDK do .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="96276-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="96276-149">Um aviso será exibido se o SDK do .NET Core sendo usado inclui a ferramenta:</span><span class="sxs-lookup"><span data-stu-id="96276-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="96276-150">Instalar o [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacote NuGet em seu projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96276-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="96276-151">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="96276-151">For example:</span></span>

<span data-ttu-id="96276-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="96276-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="96276-153">Execute o seguinte comando em um shell de comando para validar a instalação da ferramenta:</span><span class="sxs-lookup"><span data-stu-id="96276-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="96276-154">A ferramenta Gerenciador de segredo exibe a Ajuda do comando de exemplo de uso e opções:</span><span class="sxs-lookup"><span data-stu-id="96276-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="96276-155">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="96276-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="96276-156">Definir um segredo</span><span class="sxs-lookup"><span data-stu-id="96276-156">Set a secret</span></span>

<span data-ttu-id="96276-157">A ferramenta Gerenciador de segredo opera em específica do projeto configurações armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="96276-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="96276-158">Para usar os segredos do usuário, defina um `UserSecretsId` elemento dentro de um `PropertyGroup` do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="96276-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="96276-159">O valor de `UserSecretsId` é arbitrária, mas é exclusiva ao projeto.</span><span class="sxs-lookup"><span data-stu-id="96276-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="96276-160">Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="96276-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="96276-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="96276-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="96276-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="96276-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="96276-163">No Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="96276-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="96276-164">Esse gesto adiciona um `UserSecretsId` elemento, preenchido com um GUID para o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="96276-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="96276-165">O Visual Studio abrirá um *secrets.json* arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="96276-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="96276-166">Substitua o conteúdo do *secrets.json* com os pares chave-valor a ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="96276-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="96276-167">Por exemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="96276-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="96276-168">Defina um segredo do aplicativo consiste em uma chave e seu valor.</span><span class="sxs-lookup"><span data-stu-id="96276-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="96276-169">O segredo é associado ao projeto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="96276-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="96276-170">Por exemplo, execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="96276-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="96276-171">No exemplo anterior, os dois-pontos indica que `Movies` é um objeto literal com um `ServiceApiKey` propriedade.</span><span class="sxs-lookup"><span data-stu-id="96276-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="96276-172">A ferramenta Gerenciador de segredo pode ser usada de outros diretórios muito.</span><span class="sxs-lookup"><span data-stu-id="96276-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="96276-173">Use o `--project` opção de fornecer o caminho do sistema de arquivos no qual o *. csproj* arquivo existe.</span><span class="sxs-lookup"><span data-stu-id="96276-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="96276-174">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="96276-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="96276-175">Definir vários segredos</span><span class="sxs-lookup"><span data-stu-id="96276-175">Set multiple secrets</span></span>

<span data-ttu-id="96276-176">Um lote de segredos pode ser definido ao canalizar JSON para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="96276-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="96276-177">No exemplo a seguir, o *input.json* conteúdo do arquivo é direcionado para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="96276-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="96276-178">Windows</span><span class="sxs-lookup"><span data-stu-id="96276-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="96276-179">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="96276-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="96276-180">macOS</span><span class="sxs-lookup"><span data-stu-id="96276-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="96276-181">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="96276-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="96276-182">Linux</span><span class="sxs-lookup"><span data-stu-id="96276-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="96276-183">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="96276-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="96276-184">Acessar um segredo</span><span class="sxs-lookup"><span data-stu-id="96276-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="96276-185">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso para segredos do Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="96276-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="96276-186">Instalar o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="96276-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="96276-187">Adicione a fonte de configuração de segredos do usuário com uma chamada para [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="96276-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="96276-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="96276-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="96276-189">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso para segredos do Gerenciador de segredo.</span><span class="sxs-lookup"><span data-stu-id="96276-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="96276-190">Se seu projeto direcionado ao .NET Framework, instale o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="96276-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="96276-191">No ASP.NET Core 2.0 ou posterior, a fonte de configuração de segredos do usuário é adicionada automaticamente em modo de desenvolvimento quando o projeto chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar uma nova instância do host com padrões pré-configurado.</span><span class="sxs-lookup"><span data-stu-id="96276-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="96276-192">`CreateDefaultBuilder` chamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) quando o [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) é [desenvolvimento](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="96276-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="96276-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="96276-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="96276-194">Quando `CreateDefaultBuilder` não é chamado durante a construção de host, adicione a fonte de configuração de segredos do usuário com uma chamada para [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="96276-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="96276-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="96276-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="96276-196">Segredos do usuário podem ser recuperados por meio de `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="96276-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="96276-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="96276-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="96276-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="96276-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="96276-199">Substituição de cadeia de caracteres com segredos</span><span class="sxs-lookup"><span data-stu-id="96276-199">String replacement with secrets</span></span>

<span data-ttu-id="96276-200">Armazenar senhas em texto sem formatação é insegura.</span><span class="sxs-lookup"><span data-stu-id="96276-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="96276-201">Por exemplo, uma cadeia de caracteres de conexão do banco de dados armazenado em *appSettings. JSON* pode incluir uma senha para o usuário especificado:</span><span class="sxs-lookup"><span data-stu-id="96276-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="96276-202">Uma abordagem mais segura é armazenar a senha como um segredo.</span><span class="sxs-lookup"><span data-stu-id="96276-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="96276-203">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="96276-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="96276-204">Remover o `Password` par chave-valor da cadeia de conexão no *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="96276-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="96276-205">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="96276-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="96276-206">Valor do segredo pode ser definida em um [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) do objeto [senha](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propriedade para concluir a cadeia de caracteres de conexão:</span><span class="sxs-lookup"><span data-stu-id="96276-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="96276-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="96276-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="96276-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="96276-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="96276-209">Lista os segredos</span><span class="sxs-lookup"><span data-stu-id="96276-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="96276-210">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="96276-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="96276-211">A seguinte saída é exibida:</span><span class="sxs-lookup"><span data-stu-id="96276-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="96276-212">No exemplo anterior, dois-pontos em nomes de chave indica que a hierarquia de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="96276-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="96276-213">Remover um segredo único</span><span class="sxs-lookup"><span data-stu-id="96276-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="96276-214">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="96276-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="96276-215">O aplicativo *secrets.json* arquivo foi modificado para remover o par chave-valor associado a `MoviesConnectionString` chave:</span><span class="sxs-lookup"><span data-stu-id="96276-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="96276-216">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="96276-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="96276-217">Remover todos os segredos</span><span class="sxs-lookup"><span data-stu-id="96276-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="96276-218">Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="96276-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="96276-219">Todos os segredos do usuário para o aplicativo tem sido excluídos do *secrets.json* arquivo:</span><span class="sxs-lookup"><span data-stu-id="96276-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="96276-220">Executando `dotnet user-secrets list` exibirá a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="96276-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="96276-221">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="96276-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
