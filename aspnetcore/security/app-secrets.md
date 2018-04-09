---
title: Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET
author: rick-anderson
description: Mostra como armazenar com segurança os segredos durante o desenvolvimento
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 166111696a9c4244ede44fca8878dd3725bb3099
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="c7be0-103">Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7be0-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="c7be0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c7be0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="c7be0-105">Este documento mostra como você pode usar a ferramenta Gerenciador de segredo em desenvolvimento para manter os segredos fora do seu código.</span><span class="sxs-lookup"><span data-stu-id="c7be0-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="c7be0-106">O ponto mais importante é que você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar os segredos de produção no modo de desenvolvimento e teste.</span><span class="sxs-lookup"><span data-stu-id="c7be0-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="c7be0-107">Em vez disso, você pode usar o [configuração](xref:fundamentals/configuration/index) sistema ler esses valores de variáveis de ambiente ou dos valores armazenados usando o Gerenciador de segredo de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="c7be0-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="c7be0-108">A ferramenta Gerenciador de segredo ajuda a impedir que os dados confidenciais que está sendo verificado no controle de origem.</span><span class="sxs-lookup"><span data-stu-id="c7be0-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="c7be0-109">O [configuração](xref:fundamentals/configuration/index) sistema pode ler os segredos armazenados com a ferramenta Gerenciador de segredo descrita neste artigo.</span><span class="sxs-lookup"><span data-stu-id="c7be0-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="c7be0-110">A ferramenta Gerenciador de segredo é usada somente em desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c7be0-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="c7be0-111">Você pode proteger segredos de teste e produção do Azure com o [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provedor de configuração.</span><span class="sxs-lookup"><span data-stu-id="c7be0-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="c7be0-112">Consulte [provedor de configuração do Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="c7be0-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="c7be0-113">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="c7be0-113">Environment variables</span></span>

<span data-ttu-id="c7be0-114">Para evitar armazenar segredos do aplicativo no código ou em arquivos de configuração local, você pode armazenar segredos em variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="c7be0-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="c7be0-115">Você pode configurar o [configuração](xref:fundamentals/configuration/index) framework para ler valores de variáveis de ambiente chamando `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="c7be0-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="c7be0-116">Você pode usar variáveis de ambiente para substituir valores de configuração para todas as fontes de configuração especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c7be0-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="c7be0-117">Por exemplo, se você criar um novo aplicativo web ASP.NET Core com contas de usuário individuais, ele irá adicionar uma cadeia de caracteres de conexão padrão para o *appSettings. JSON* arquivo no projeto com a chave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="c7be0-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="c7be0-118">A cadeia de caracteres de conexão padrão está configurado para usar LocalDB, que é executado no modo de usuário e não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="c7be0-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="c7be0-119">Quando você implanta seu aplicativo em um servidor de teste ou de produção, você pode substituir o `DefaultConnection` valor de chave com uma configuração de variável de ambiente que contém a cadeia de caracteres de conexão (possivelmente com credenciais confidenciais) para um banco de dados de teste ou de produção servidor.</span><span class="sxs-lookup"><span data-stu-id="c7be0-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="c7be0-120">Variáveis de ambiente geralmente são armazenadas em texto sem formatação e não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="c7be0-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="c7be0-121">Se o computador ou o processo estiver comprometido, variáveis de ambiente podem ser acessadas por indivíduos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="c7be0-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="c7be0-122">Medidas adicionais para evitar a divulgação de segredos do usuário ainda podem ser necessárias.</span><span class="sxs-lookup"><span data-stu-id="c7be0-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="c7be0-123">Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="c7be0-123">Secret Manager</span></span>

<span data-ttu-id="c7be0-124">A ferramenta Gerenciador de segredo armazena dados confidenciais para o trabalho de desenvolvimento fora da árvore do projeto.</span><span class="sxs-lookup"><span data-stu-id="c7be0-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="c7be0-125">A ferramenta Gerenciador de segredo é uma ferramenta de projeto que pode ser usada para armazenar segredos para um projeto .NET Core durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c7be0-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="c7be0-126">Com a ferramenta Gerenciador de segredo, você pode associar os segredos do aplicativo um projeto específico e compartilhá-los em vários projetos.</span><span class="sxs-lookup"><span data-stu-id="c7be0-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="c7be0-127">A ferramenta Gerenciador de segredo não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável.</span><span class="sxs-lookup"><span data-stu-id="c7be0-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="c7be0-128">Ele destina-se apenas para fins de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c7be0-128">It's for development purposes only.</span></span> <span data-ttu-id="c7be0-129">As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="c7be0-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="c7be0-130">Instalando a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="c7be0-130">Installing the Secret Manager tool</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7be0-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7be0-131">Visual Studio</span></span>](#tab/visual-studio/)
<span data-ttu-id="c7be0-132">Clique com botão direito no projeto no Gerenciador de soluções e selecione **editar \<project_name\>. csproj** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="c7be0-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="c7be0-133">Adicione a linha realçada para o *. csproj* de arquivo e salvar para restaurar o pacote NuGet associado:</span><span class="sxs-lookup"><span data-stu-id="c7be0-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="c7be0-134">Clique com botão direito no projeto no Gerenciador de soluções novamente e selecione **gerenciar segredos do usuário** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="c7be0-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="c7be0-135">Esse gesto adiciona um novo `UserSecretsId` nó dentro de um `PropertyGroup` do *. csproj* arquivo, como destacado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="c7be0-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="c7be0-136">Salvando o *. csproj* arquivo também abre uma `secrets.json` arquivo no editor de texto.</span><span class="sxs-lookup"><span data-stu-id="c7be0-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="c7be0-137">Substitua o conteúdo do `secrets.json` arquivo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c7be0-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c7be0-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c7be0-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)
<span data-ttu-id="c7be0-139">Adicionar `Microsoft.Extensions.SecretManager.Tools` para o *. csproj* e execute o [restauração dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="c7be0-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="c7be0-140">Você pode usar as mesmas etapas para instalar a ferramenta Gerenciador de segredo usando a linha de comando.</span><span class="sxs-lookup"><span data-stu-id="c7be0-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="c7be0-141">Teste a ferramenta Gerenciador de segredo executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c7be0-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="c7be0-142">A ferramenta Gerenciador de segredo exibirá uso, opções e a Ajuda do comando.</span><span class="sxs-lookup"><span data-stu-id="c7be0-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="c7be0-143">Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` nós.</span><span class="sxs-lookup"><span data-stu-id="c7be0-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="c7be0-144">A ferramenta Gerenciador de segredo opera em definições de configuração específicas de projeto que são armazenadas no perfil do usuário.</span><span class="sxs-lookup"><span data-stu-id="c7be0-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="c7be0-145">Para usar os segredos do usuário, o projeto deve especificar um `UserSecretsId` valor em seu *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c7be0-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="c7be0-146">O valor de `UserSecretsId` é arbitrária, mas é geralmente exclusivo para o projeto.</span><span class="sxs-lookup"><span data-stu-id="c7be0-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="c7be0-147">Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="c7be0-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="c7be0-148">Adicionar um `UserSecretsId` para seu projeto no *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="c7be0-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="c7be0-149">Use a ferramenta Gerenciador de segredo para definir um segredo.</span><span class="sxs-lookup"><span data-stu-id="c7be0-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="c7be0-150">Por exemplo, em uma janela de comando do diretório do projeto, digite o seguinte:</span><span class="sxs-lookup"><span data-stu-id="c7be0-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="c7be0-151">Você pode executar a ferramenta Gerenciador de segredo de outros diretórios, mas você deve usar o `--project` opção para passar o caminho para o *. csproj* arquivo:</span><span class="sxs-lookup"><span data-stu-id="c7be0-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="c7be0-152">Você também pode usar a ferramenta Gerenciador de segredo de lista, remover e limpar os segredos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c7be0-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

* * *
## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="c7be0-153">Acessando os segredos do usuário por meio da configuração</span><span class="sxs-lookup"><span data-stu-id="c7be0-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="c7be0-154">Acessar o Gerenciador de segredo segredos através do sistema de configuração.</span><span class="sxs-lookup"><span data-stu-id="c7be0-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="c7be0-155">Adicionar o `Microsoft.Extensions.Configuration.UserSecrets` empacotar e executar [restauração dotnet](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="c7be0-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="c7be0-156">Adicione a fonte de configuração de segredos do usuário para o `Startup` método:</span><span class="sxs-lookup"><span data-stu-id="c7be0-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="c7be0-157">Você pode acessar os segredos do usuário via a API de configuração:</span><span class="sxs-lookup"><span data-stu-id="c7be0-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="c7be0-158">Como funciona a ferramenta Gerenciador de segredo</span><span class="sxs-lookup"><span data-stu-id="c7be0-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="c7be0-159">A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados.</span><span class="sxs-lookup"><span data-stu-id="c7be0-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="c7be0-160">Você pode usar a ferramenta sem conhecer esses detalhes de implementação.</span><span class="sxs-lookup"><span data-stu-id="c7be0-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="c7be0-161">Na versão atual, os valores são armazenados em um [JSON](http://json.org/) arquivo de configuração no diretório de perfil do usuário:</span><span class="sxs-lookup"><span data-stu-id="c7be0-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="c7be0-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c7be0-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="c7be0-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c7be0-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="c7be0-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="c7be0-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="c7be0-165">O valor de `userSecretsId` vem do valor especificado em *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="c7be0-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="c7be0-166">Você não deve escrever código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo, como esses detalhes de implementação podem ser alterado.</span><span class="sxs-lookup"><span data-stu-id="c7be0-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="c7be0-167">Por exemplo, os valores secretos são atualmente *não* criptografado hoje, mas pode ser um dia.</span><span class="sxs-lookup"><span data-stu-id="c7be0-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7be0-168">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c7be0-168">Additional resources</span></span>

* [<span data-ttu-id="c7be0-169">Configuração</span><span class="sxs-lookup"><span data-stu-id="c7be0-169">Configuration</span></span>](xref:fundamentals/configuration/index)
