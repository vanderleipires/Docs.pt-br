---
title: Armazenamento seguro de segredos do aplicativo durante developmentin ASP.NET Core
author: rick-anderson
description: "Mostra como armazenar com segurança os segredos durante o desenvolvimento"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 56214c2fbdca84591c5c1a6b7f2451f33ee64ef0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="f7074-104">Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7074-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<a name=security-app-secrets></a>

<span data-ttu-id="f7074-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="f7074-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="f7074-106">Este documento mostra como você pode usar a ferramenta Gerenciador de segredo em desenvolvimento para manter os segredos fora do seu código.</span><span class="sxs-lookup"><span data-stu-id="f7074-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="f7074-107">O ponto mais importante é que você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar os segredos de produção no modo de desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="f7074-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="f7074-108">Em vez disso, você pode usar o [configuração](../fundamentals/configuration.md) sistema ler esses valores de variáveis de ambiente ou dos valores armazenados usando o Gerenciador de segredo de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="f7074-108">You can instead use the [configuration](../fundamentals/configuration.md) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="f7074-109">A ferramenta Gerenciador de segredo ajuda a impedir que os dados confidenciais que está sendo verificado no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="f7074-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="f7074-110">O [configuração](../fundamentals/configuration.md) sistema pode ler os segredos armazenados com a ferramenta Gerenciador de segredo descrita neste artigo.</span><span class="sxs-lookup"><span data-stu-id="f7074-110">The [configuration](../fundamentals/configuration.md) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="f7074-111">A ferramenta Gerenciador de segredo é usada somente em desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f7074-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="f7074-112">Você pode proteger segredos de teste e produção do Azure com o [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provedor de configuração.</span><span class="sxs-lookup"><span data-stu-id="f7074-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="f7074-113">Consulte [provedor de configuração do Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="f7074-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f7074-114">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="f7074-114">Environment variables</span></span>

<span data-ttu-id="f7074-115">Para evitar armazenar segredos do aplicativo no código ou em arquivos de configuração local, você pode armazenar segredos em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="f7074-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="f7074-116">Você pode configurar o [configuração](../fundamentals/configuration.md) framework para ler valores de variáveis de ambiente chamando `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f7074-116">You can setup the [configuration](../fundamentals/configuration.md) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="f7074-117">Você pode usar variáveis de ambiente para substituir valores de configuração para todas as fontes de configuração especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f7074-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="f7074-118">Por exemplo, se você criar um novo aplicativo web ASP.NET Core com contas de usuário individuais, ele irá adicionar uma cadeia de caracteres de conexão padrão para o *appSettings. JSON* arquivo no projeto com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f7074-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="f7074-119">A cadeia de caracteres de conexão padrão está configurado para usar LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="f7074-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f7074-120">Quando você implanta seu aplicativo em um servidor de teste ou de produção, você pode substituir o `DefaultConnection` valor de chave com uma configuração de variável de ambiente que contém a cadeia de caracteres de conexão (possivelmente com credenciais confidenciais) para um banco de dados de teste ou de produção servidor.</span><span class="sxs-lookup"><span data-stu-id="f7074-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="f7074-121">Variáveis de ambiente geralmente são armazenadas em texto sem formatação e não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="f7074-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="f7074-122">Se o computador ou o processo estiver comprometido, variáveis de ambiente podem ser acessadas por indivíduos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="f7074-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f7074-123">Medidas adicionais para evitar a divulgação de segredos do usuário ainda podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="f7074-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="f7074-124">Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="f7074-124">Secret Manager</span></span>

<span data-ttu-id="f7074-125">A ferramenta Gerenciador de segredo armazena dados confidenciais para o trabalho de desenvolvimento fora da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="f7074-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="f7074-126">A ferramenta Gerenciador de segredo é uma ferramenta de projeto que pode ser usada para armazenar segredos para um [.NET Core](https://www.microsoft.com/net/core) projeto durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f7074-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="f7074-127">Com a ferramenta Gerenciador de segredo, você pode associar os segredos do aplicativo um projeto específico e compartilhá-los em vários projetos.</span><span class="sxs-lookup"><span data-stu-id="f7074-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="f7074-128">A ferramenta Gerenciador de segredo não criptografar os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="f7074-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="f7074-129">Ele destina-se apenas para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f7074-129">It is for development purposes only.</span></span> <span data-ttu-id="f7074-130">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="f7074-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

### <a name="visual-studio-2017-installing-the-secret-manager-tool"></a><span data-ttu-id="f7074-131">Visual Studio de 2017: Instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="f7074-131">Visual Studio 2017: Installing the Secret Manager tool</span></span>

<span data-ttu-id="f7074-132">Clique com botão direito no projeto no Gerenciador de soluções e selecione **editar \<project_name\>. csproj** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f7074-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="f7074-133">Adicione a linha realçada para o *. csproj* de arquivo e salvar para restaurar o pacote NuGet associado:</span><span class="sxs-lookup"><span data-stu-id="f7074-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

<span data-ttu-id="f7074-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="f7074-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="f7074-135">Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f7074-135">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f7074-136">Esse gesto adiciona um novo `UserSecretsId` nó dentro de um `PropertyGroup` do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f7074-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f7074-137">Ele também abre uma `secrets.json` arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="f7074-137">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="f7074-138">Adicione o seguinte `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="f7074-138">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-2015-installing-the-secret-manager-tool"></a><span data-ttu-id="f7074-139">Visual Studio 2015: Instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="f7074-139">Visual Studio 2015: Installing the Secret Manager tool</span></span>

<span data-ttu-id="f7074-140">Abra o projeto `project.json` arquivo.</span><span class="sxs-lookup"><span data-stu-id="f7074-140">Open the project's `project.json` file.</span></span> <span data-ttu-id="f7074-141">Adicione uma referência a `Microsoft.Extensions.SecretManager.Tools` dentro do `tools` propriedade e salvar para restaurar o pacote NuGet associado:</span><span class="sxs-lookup"><span data-stu-id="f7074-141">Add a reference to `Microsoft.Extensions.SecretManager.Tools` within the `tools` property, and save to restore the associated NuGet package:</span></span>

```json
"tools": {
    "Microsoft.Extensions.SecretManager.Tools": "1.0.0-preview2-final",
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
},
```

<span data-ttu-id="f7074-142">Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f7074-142">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f7074-143">Esse gesto adiciona um novo `userSecretsId` propriedade `project.json`.</span><span class="sxs-lookup"><span data-stu-id="f7074-143">This gesture adds a new `userSecretsId` property to `project.json`.</span></span> <span data-ttu-id="f7074-144">Ele também abre uma `secrets.json` arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="f7074-144">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="f7074-145">Adicione o seguinte `secrets.json`:</span><span class="sxs-lookup"><span data-stu-id="f7074-145">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-code-or-command-line-installing-the-secret-manager-tool"></a><span data-ttu-id="f7074-146">Visual Studio Code ou linha de comando: instalar a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="f7074-146">Visual Studio Code or Command Line: Installing the Secret Manager tool</span></span>

<span data-ttu-id="f7074-147">Adicionar `Microsoft.Extensions.SecretManager.Tools` para o *. csproj* e execute o `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="f7074-147">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span>

<span data-ttu-id="f7074-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="f7074-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="f7074-149">Teste a ferramenta Gerenciador de segredo executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="f7074-149">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="f7074-150">A ferramenta Gerenciador de segredo exibirá uso, opções e a Ajuda do comando.</span><span class="sxs-lookup"><span data-stu-id="f7074-150">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="f7074-151">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` nós.</span><span class="sxs-lookup"><span data-stu-id="f7074-151">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="f7074-152">A ferramenta Gerenciador de segredo opera em definições de configuração específicas de projeto que são armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="f7074-152">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="f7074-153">Para usar os segredos do usuário, o projeto deve especificar um `UserSecretsId` valor em seu *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f7074-153">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="f7074-154">O valor de `UserSecretsId` é arbitrária, mas é geralmente exclusivo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="f7074-154">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="f7074-155">Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f7074-155">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="f7074-156">Adicionar um `UserSecretsId` para seu projeto no *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="f7074-156">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

<span data-ttu-id="f7074-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="f7074-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span></span>

<span data-ttu-id="f7074-158">Use a ferramenta Gerenciador de segredo para definir um segredo.</span><span class="sxs-lookup"><span data-stu-id="f7074-158">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="f7074-159">Por exemplo, em uma janela de comando do diretório do projeto, digite o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f7074-159">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="f7074-160">Você pode executar a ferramenta Gerenciador de segredo de outros diretórios, mas você deve usar o `--project` opção para passar o caminho para o *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="f7074-160">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="f7074-161">Você também pode usar a ferramenta Gerenciador de segredo de lista, remover e limpar os segredos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f7074-161">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="f7074-162">Acessando os segredos do usuário por meio da configuração</span><span class="sxs-lookup"><span data-stu-id="f7074-162">Accessing user secrets via configuration</span></span>

<span data-ttu-id="f7074-163">Acessar o Gerenciador de segredo segredos através do sistema de configuração.</span><span class="sxs-lookup"><span data-stu-id="f7074-163">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="f7074-164">Adicionar o `Microsoft.Extensions.Configuration.UserSecrets` empacotar e executar `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="f7074-164">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="f7074-165">Adicione a fonte de configuração de segredos do usuário para o `Startup` método:</span><span class="sxs-lookup"><span data-stu-id="f7074-165">Add the user secrets configuration source to the `Startup` method:</span></span>

<span data-ttu-id="f7074-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span><span class="sxs-lookup"><span data-stu-id="f7074-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span></span>

<span data-ttu-id="f7074-167">Você pode acessar os segredos do usuário via a API de configuração:</span><span class="sxs-lookup"><span data-stu-id="f7074-167">You can access user secrets via the configuration API:</span></span>

<span data-ttu-id="f7074-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="f7074-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f7074-169">Como funciona a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="f7074-169">How the Secret Manager tool works</span></span>

<span data-ttu-id="f7074-170">A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="f7074-170">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f7074-171">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="f7074-171">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f7074-172">Na versão atual, os valores são armazenados em um [JSON](http://json.org/) arquivo de configuração no diretório de perfil do usuário:</span><span class="sxs-lookup"><span data-stu-id="f7074-172">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="f7074-173">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="f7074-173">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="f7074-174">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="f7074-174">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="f7074-175">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="f7074-175">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="f7074-176">O valor de `userSecretsId` vem do valor especificado em *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="f7074-176">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="f7074-177">Você não deve gravar o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo, como esses detalhes de implementação podem ser alterado.</span><span class="sxs-lookup"><span data-stu-id="f7074-177">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="f7074-178">Por exemplo, os valores secretos são atualmente *não* criptografado hoje, mas pode ser um dia.</span><span class="sxs-lookup"><span data-stu-id="f7074-178">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7074-179">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="f7074-179">Additional Resources</span></span>

* [<span data-ttu-id="f7074-180">Configuração</span><span class="sxs-lookup"><span data-stu-id="f7074-180">Configuration</span></span>](../fundamentals/configuration.md)
