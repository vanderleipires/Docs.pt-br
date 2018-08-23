---
title: Armazenamento seguro dos segredos do aplicativo em desenvolvimento no ASP.NET Core
author: rick-anderson
description: Saiba como armazenar e recuperar informações confidenciais como segredos do aplicativo durante o desenvolvimento de um aplicativo ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41833770"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="e39b9-103">Armazenamento seguro dos segredos do aplicativo em desenvolvimento no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e39b9-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="e39b9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e39b9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e39b9-105">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e39b9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e39b9-106">Este documento explica as técnicas para armazenar e recuperar dados confidenciais durante o desenvolvimento de um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e39b9-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="e39b9-107">Você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar segredos de produção em desenvolvimento ou modo de teste.</span><span class="sxs-lookup"><span data-stu-id="e39b9-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="e39b9-108">Você pode armazenar e proteger os segredos de teste e produção do Azure com o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="e39b9-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="e39b9-109">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="e39b9-109">Environment variables</span></span>

<span data-ttu-id="e39b9-110">Variáveis de ambiente são usadas para evitar o armazenamento de segredos do aplicativo no código ou em arquivos de configuração local.</span><span class="sxs-lookup"><span data-stu-id="e39b9-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="e39b9-111">Variáveis de ambiente substituem os valores de configuração para todas as fontes de configuração especificado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="e39b9-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e39b9-112">Configure a leitura dos valores de variáveis de ambiente chamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="e39b9-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="e39b9-113">Considere um aplicativo da web ASP.NET Core no qual **contas de usuário individuais** segurança está habilitada.</span><span class="sxs-lookup"><span data-stu-id="e39b9-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="e39b9-114">Uma cadeia de caracteres de conexão de banco de dados padrão está incluída no projeto do *appSettings. JSON* arquivo com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="e39b9-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="e39b9-115">É a cadeia de caracteres de conexão padrão para o LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="e39b9-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="e39b9-116">Durante a implantação de aplicativo, o `DefaultConnection` valor de chave pode ser substituído com o valor da variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="e39b9-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="e39b9-117">A variável de ambiente pode armazenar a cadeia de caracteres de conexão completa com credenciais confidenciais.</span><span class="sxs-lookup"><span data-stu-id="e39b9-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="e39b9-118">Variáveis de ambiente geralmente são armazenadas em texto não criptografado e sem formatação.</span><span class="sxs-lookup"><span data-stu-id="e39b9-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="e39b9-119">Se o computador ou processo for comprometido, as variáveis de ambiente podem ser acessadas por pessoas não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="e39b9-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="e39b9-120">Medidas adicionais para evitar a divulgação de segredos do usuário podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="e39b9-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="e39b9-121">Secret Manager</span><span class="sxs-lookup"><span data-stu-id="e39b9-121">Secret Manager</span></span>

<span data-ttu-id="e39b9-122">A ferramenta Secret Manager armazena dados confidenciais durante o desenvolvimento de um projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e39b9-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="e39b9-123">Nesse contexto, uma parte dos dados confidenciais é um segredo do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e39b9-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="e39b9-124">Segredos do aplicativo são armazenados em um local separado da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="e39b9-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="e39b9-125">Os segredos do aplicativo são compartilhados em vários projetos ou associados a um projeto específico.</span><span class="sxs-lookup"><span data-stu-id="e39b9-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="e39b9-126">Os segredos do aplicativo não são verificados no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="e39b9-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="e39b9-127">A ferramenta Secret Manager não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="e39b9-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="e39b9-128">Ele é apenas a fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="e39b9-128">It's for development purposes only.</span></span> <span data-ttu-id="e39b9-129">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="e39b9-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="e39b9-130">Como funciona a ferramenta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="e39b9-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="e39b9-131">A ferramenta Secret Manager abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="e39b9-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="e39b9-132">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="e39b9-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="e39b9-133">Os valores são armazenados em um arquivo de configuração do JSON em uma pasta de perfil do usuário do sistema protegido no computador local:</span><span class="sxs-lookup"><span data-stu-id="e39b9-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="e39b9-134">Windows</span><span class="sxs-lookup"><span data-stu-id="e39b9-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="e39b9-135">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="e39b9-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="e39b9-136">macOS</span><span class="sxs-lookup"><span data-stu-id="e39b9-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="e39b9-137">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="e39b9-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="e39b9-138">Linux</span><span class="sxs-lookup"><span data-stu-id="e39b9-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="e39b9-139">Caminho do sistema de arquivos:</span><span class="sxs-lookup"><span data-stu-id="e39b9-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="e39b9-140">Na anterior caminhos de arquivo, substitua `<user_secrets_id>` com o `UserSecretsId` valor especificado na *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e39b9-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="e39b9-141">Não escreva código que depende do local ou o formato dos dados salvos com a ferramenta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="e39b9-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="e39b9-142">Esses detalhes de implementação pode ser alterado.</span><span class="sxs-lookup"><span data-stu-id="e39b9-142">These implementation details may change.</span></span> <span data-ttu-id="e39b9-143">Por exemplo, os valores secretos não são criptografados, mas pode ser no futuro.</span><span class="sxs-lookup"><span data-stu-id="e39b9-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="e39b9-144">Instalar a ferramenta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="e39b9-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="e39b9-145">A ferramenta Secret Manager é fornecido com a CLI do .NET Core no SDK do .NET Core 2.1.300 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="e39b9-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="e39b9-146">Para versões do SDK do .NET Core anteriores 2.1.300, a instalação da ferramenta é necessária.</span><span class="sxs-lookup"><span data-stu-id="e39b9-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="e39b9-147">Executar `dotnet --version` em um shell de comando para ver o número de versão do SDK do .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="e39b9-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="e39b9-148">Um aviso será exibido se a ferramenta inclui o SDK do .NET Core que está sendo usada:</span><span class="sxs-lookup"><span data-stu-id="e39b9-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="e39b9-149">Instalar o [secretmanager](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacote do NuGet em seu projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e39b9-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="e39b9-150">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="e39b9-151">Execute o seguinte comando em um shell de comando para validar a instalação da ferramenta:</span><span class="sxs-lookup"><span data-stu-id="e39b9-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="e39b9-152">A ferramenta Secret Manager exibe um exemplo de uso e opções de ajuda de comando:</span><span class="sxs-lookup"><span data-stu-id="e39b9-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="e39b9-153">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas na *. csproj* do arquivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="e39b9-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="e39b9-154">Defina um segredo</span><span class="sxs-lookup"><span data-stu-id="e39b9-154">Set a secret</span></span>

<span data-ttu-id="e39b9-155">A ferramenta Secret Manager opera em definições de configuração de específicos do projeto armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="e39b9-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="e39b9-156">Para usar os segredos do usuário, defina uma `UserSecretsId` elemento dentro de uma `PropertyGroup` da *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e39b9-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="e39b9-157">O valor de `UserSecretsId` é arbitrária, mas é exclusiva para o projeto.</span><span class="sxs-lookup"><span data-stu-id="e39b9-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="e39b9-158">Os desenvolvedores geralmente geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="e39b9-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="e39b9-159">No Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="e39b9-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="e39b9-160">Esse gesto adiciona uma `UserSecretsId` elemento, preenchido com um GUID para o *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="e39b9-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="e39b9-161">O Visual Studio abre uma *Secrets* arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="e39b9-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="e39b9-162">Substitua o conteúdo do *Secrets* com os pares chave-valor a ser armazenado.</span><span class="sxs-lookup"><span data-stu-id="e39b9-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="e39b9-163">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="e39b9-164">A estrutura JSON é mesclada após modificações via `dotnet user-secrets remove` ou `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="e39b9-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="e39b9-165">Por exemplo, executando `dotnet user-secrets remove "Movies:ConnectionString"` recolhe o `Movies` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="e39b9-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="e39b9-166">O arquivo modificado é semelhante a:</span><span class="sxs-lookup"><span data-stu-id="e39b9-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="e39b9-167">Defina um segredo do aplicativo consiste em uma chave e seu valor.</span><span class="sxs-lookup"><span data-stu-id="e39b9-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="e39b9-168">O segredo está associado com o projeto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="e39b9-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="e39b9-169">Por exemplo, execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="e39b9-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="e39b9-170">No exemplo anterior, os dois-pontos indica que `Movies` é um objeto literal com um `ServiceApiKey` propriedade.</span><span class="sxs-lookup"><span data-stu-id="e39b9-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="e39b9-171">A ferramenta Secret Manager pode ser usada de outros diretórios muito.</span><span class="sxs-lookup"><span data-stu-id="e39b9-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="e39b9-172">Use o `--project` opção de fornecer o caminho do sistema de arquivos no qual o *. csproj* arquivo existe.</span><span class="sxs-lookup"><span data-stu-id="e39b9-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="e39b9-173">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="e39b9-174">Defina vários segredos</span><span class="sxs-lookup"><span data-stu-id="e39b9-174">Set multiple secrets</span></span>

<span data-ttu-id="e39b9-175">Um lote de segredos pode ser definido ao canalizar o JSON para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="e39b9-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="e39b9-176">No exemplo a seguir, o *Input* conteúdo do arquivo será canalizado para o `set` comando.</span><span class="sxs-lookup"><span data-stu-id="e39b9-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="e39b9-177">Windows</span><span class="sxs-lookup"><span data-stu-id="e39b9-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="e39b9-178">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e39b9-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="e39b9-179">macOS</span><span class="sxs-lookup"><span data-stu-id="e39b9-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="e39b9-180">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e39b9-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="e39b9-181">Linux</span><span class="sxs-lookup"><span data-stu-id="e39b9-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="e39b9-182">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e39b9-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="e39b9-183">Acesse um segredo</span><span class="sxs-lookup"><span data-stu-id="e39b9-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e39b9-184">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso aos segredos Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="e39b9-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="e39b9-185">Se seu projeto direcionado ao .NET Framework, instale o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e39b9-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="e39b9-186">No ASP.NET Core 2.0 ou posterior, a fonte de configuração de segredos do usuário é adicionada automaticamente no modo de desenvolvimento quando o projeto chama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar uma nova instância do host com os padrões pré-configurados.</span><span class="sxs-lookup"><span data-stu-id="e39b9-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="e39b9-187">`CreateDefaultBuilder` chamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) quando o [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) está [desenvolvimento](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="e39b9-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="e39b9-188">Quando `CreateDefaultBuilder` não é chamado durante a construção de host, adicione a fonte de configuração de segredos do usuário com uma chamada para [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="e39b9-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="e39b9-189">O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso aos segredos Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="e39b9-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="e39b9-190">Instalar o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e39b9-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="e39b9-191">Adicionar a fonte de configuração de segredos do usuário com uma chamada para [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) no `Startup` construtor:</span><span class="sxs-lookup"><span data-stu-id="e39b9-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="e39b9-192">Segredos do usuário podem ser recuperados por meio de `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="e39b9-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="e39b9-193">Segredos do mapa para um POCO</span><span class="sxs-lookup"><span data-stu-id="e39b9-193">Map secrets to a POCO</span></span>

<span data-ttu-id="e39b9-194">Mapeando um literal de objeto inteiro para um POCO (uma classe .NET simple com propriedades) é útil para agregar as propriedades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="e39b9-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="e39b9-195">Para mapear os segredos anteriores para um POCO, use o `Configuration` da API [de associação do grafo de objeto](xref:fundamentals/configuration/index#bind-to-an-object-graph) recurso.</span><span class="sxs-lookup"><span data-stu-id="e39b9-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="e39b9-196">O código a seguir associa a um personalizado `MovieSettings` POCO e acessa o `ServiceApiKey` valor da propriedade:</span><span class="sxs-lookup"><span data-stu-id="e39b9-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="e39b9-197">O `Movies:ConnectionString` e `Movies:ServiceApiKey` segredos são mapeados para as respectivas propriedades no `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="e39b9-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="e39b9-198">Cadeia de caracteres de substituição com segredos</span><span class="sxs-lookup"><span data-stu-id="e39b9-198">String replacement with secrets</span></span>

<span data-ttu-id="e39b9-199">Armazenar senhas em texto sem formatação é inseguro.</span><span class="sxs-lookup"><span data-stu-id="e39b9-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="e39b9-200">Por exemplo, uma cadeia de caracteres de conexão de banco de dados armazenados em *appSettings. JSON* pode incluir uma senha para o usuário especificado:</span><span class="sxs-lookup"><span data-stu-id="e39b9-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="e39b9-201">Uma abordagem mais segura é armazenar a senha como um segredo.</span><span class="sxs-lookup"><span data-stu-id="e39b9-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="e39b9-202">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="e39b9-203">Remover o `Password` par chave-valor da cadeia de conexão na *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="e39b9-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="e39b9-204">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="e39b9-205">Valor do segredo pode ser definida em um [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) do objeto [senha](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propriedade para concluir a cadeia de caracteres de conexão:</span><span class="sxs-lookup"><span data-stu-id="e39b9-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="e39b9-206">Lista os segredos</span><span class="sxs-lookup"><span data-stu-id="e39b9-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="e39b9-207">Execute o seguinte comando no diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="e39b9-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="e39b9-208">A saída a seguir será exibida:</span><span class="sxs-lookup"><span data-stu-id="e39b9-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="e39b9-209">No exemplo anterior, dois-pontos em nomes de chave denota a hierarquia de objetos dentro *Secrets*.</span><span class="sxs-lookup"><span data-stu-id="e39b9-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="e39b9-210">Remover um segredo único</span><span class="sxs-lookup"><span data-stu-id="e39b9-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="e39b9-211">Execute o seguinte comando no diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="e39b9-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="e39b9-212">O aplicativo *Secrets* arquivo foi modificado para remover o par chave-valor associado a `MoviesConnectionString` chave:</span><span class="sxs-lookup"><span data-stu-id="e39b9-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="e39b9-213">Executando `dotnet user-secrets list` exibe a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="e39b9-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="e39b9-214">Remover todos os segredos</span><span class="sxs-lookup"><span data-stu-id="e39b9-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="e39b9-215">Execute o seguinte comando no diretório no qual o *. csproj* arquivo existe:</span><span class="sxs-lookup"><span data-stu-id="e39b9-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="e39b9-216">Todos os segredos do usuário para o aplicativo tem sido excluídos do *Secrets* arquivo:</span><span class="sxs-lookup"><span data-stu-id="e39b9-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="e39b9-217">Executando `dotnet user-secrets list` exibe a seguinte mensagem:</span><span class="sxs-lookup"><span data-stu-id="e39b9-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="e39b9-218">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e39b9-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
