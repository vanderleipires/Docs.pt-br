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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Armazenamento seguro de segredos do aplicativo em desenvolvimento no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([como baixar](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Este documento explica técnicas para armazenar e recuperar dados confidenciais durante o desenvolvimento de um aplicativo ASP.NET Core. Você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar segredos de produção em desenvolvimento ou modo de teste. Você pode armazenar e proteger segredos de teste e produção do Azure com o [provedor de configuração do Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variáveis de ambiente

Variáveis de ambiente são usadas para evitar o armazenamento de segredos do aplicativo no código ou em arquivos de configuração local. Variáveis de ambiente substituem os valores de configuração para todas as fontes de configuração especificada anteriormente.

::: moniker range="<= aspnetcore-1.1"
Configurar a leitura dos valores de variável de ambiente chamando [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) no `Startup` construtor:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Considere um aplicativo da web ASP.NET Core no qual **contas de usuário individuais** segurança está habilitada. Uma cadeia de caracteres de conexão de banco de dados padrão está incluída no projeto de *appSettings. JSON* arquivo com a chave `DefaultConnection`. A cadeia de caracteres de conexão padrão é para o LocalDB, que é executado no modo de usuário e não requer uma senha. Durante a implantação do aplicativo, o `DefaultConnection` valor chave pode ser substituído pelo valor da variável de ambiente. A variável de ambiente pode armazenar a cadeia de caracteres de conexão completa com credenciais confidenciais.

> [!WARNING]
> Variáveis de ambiente são geralmente armazenadas em texto sem formatação e não criptografado. Se o computador ou o processo estiver comprometido, as variáveis de ambiente podem ser acessadas por indivíduos não confiáveis. Medidas adicionais para evitar a divulgação de segredos do usuário podem ser necessárias.

## <a name="secret-manager"></a>Gerenciador de segredo

A ferramenta Gerenciador de segredo armazena dados confidenciais durante o desenvolvimento de um projeto do ASP.NET Core. Nesse contexto, uma parte dos dados confidenciais é um segredo do aplicativo. Segredos do aplicativo são armazenados em um local separado da árvore do projeto. Os segredos do aplicativo são compartilhados entre vários projetos ou associados a um projeto específico. Os segredos do aplicativo não são verificados no controle de origem.

> [!WARNING]
> A ferramenta Gerenciador de segredo não criptografa os segredos armazenados e não deve ser tratada como um repositório confiável. Ele destina-se apenas para fins de desenvolvimento. As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.

## <a name="how-the-secret-manager-tool-works"></a>Como funciona a ferramenta Gerenciador de segredo

A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados. Você pode usar a ferramenta sem conhecer esses detalhes de implementação. Os valores são armazenados em um [JSON](https://json.org/) arquivo de configuração em uma pasta de perfil de usuário protegido pelo sistema no computador local:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Caminho do sistema de arquivos:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Caminho do sistema de arquivos:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Caminho do sistema de arquivos:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Na anterior caminhos de arquivo, substitua `<user_secrets_id>` com o `UserSecretsId` valor especificado no *. csproj* arquivo.

Não grave o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo. Esses detalhes de implementação podem ser alterado. Por exemplo, os valores de segredo não estão criptografados, mas podem ser no futuro.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Instalar a ferramenta Gerenciador de segredo

A ferramenta Gerenciador de segredo é fornecida com a CLI do núcleo do .NET no .NET Core SDK 2.1. Para .NET Core SDK 2.0 e versões anteriores, a instalação da ferramenta é necessária.

Instalar o [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pacote NuGet em seu projeto do ASP.NET Core:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Execute o seguinte comando em um shell de comando para validar a instalação da ferramenta:

```console
dotnet user-secrets -h
```

A ferramenta Gerenciador de segredo exibe a Ajuda do comando de exemplo de uso e opções:

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
> Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` elementos.
::: moniker-end

## <a name="set-a-secret"></a>Definir um segredo

A ferramenta Gerenciador de segredo opera em específica do projeto configurações armazenadas no perfil do usuário. Para usar os segredos do usuário, defina um `UserSecretsId` elemento dentro de um `PropertyGroup` do *. csproj* arquivo. O valor de `UserSecretsId` é arbitrária, mas é exclusiva ao projeto. Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> No Visual Studio, clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar segredos do usuário** no menu de contexto. Esse gesto adiciona um `UserSecretsId` elemento, preenchido com um GUID para o *. csproj* arquivo. O Visual Studio abrirá um *secrets.json* arquivo no editor de texto. Substitua o conteúdo do *secrets.json* com os pares chave-valor a ser armazenado. Por exemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Defina um segredo do aplicativo consiste em uma chave e seu valor. O segredo é associado ao projeto `UserSecretsId` valor. Por exemplo, execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

No exemplo anterior, os dois-pontos indica que `Movies` é um objeto literal com um `ServiceApiKey` propriedade.

A ferramenta Gerenciador de segredo pode ser usada de outros diretórios muito. Use o `--project` opção de fornecer o caminho do sistema de arquivos no qual o *. csproj* arquivo existe. Por exemplo:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Definir vários segredos

Um lote de segredos pode ser definido ao canalizar JSON para o `set` comando. No exemplo a seguir, o *input.json* conteúdo do arquivo é direcionado para o `set` comando.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Abra um shell de comando e execute o seguinte comando:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Abra um shell de comando e execute o seguinte comando:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Abra um shell de comando e execute o seguinte comando:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Acessar um segredo

O [API de configuração do ASP.NET Core](xref:fundamentals/configuration/index) fornece acesso para segredos do Gerenciador de segredo. Se direcionando o .NET Core 1. x ou do .NET Framework, instale o [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pacote NuGet.

::: moniker range="<= aspnetcore-1.1"
Adicione a fonte de configuração de segredos do usuário para o `Startup` construtor:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Segredos do usuário podem ser recuperados por meio de `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Substituição de cadeia de caracteres com segredos

Armazenar senhas em texto sem formatação é arriscado. Por exemplo, uma cadeia de caracteres de conexão do banco de dados armazenado em *appSettings. JSON* pode incluir uma senha para o usuário especificado:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Uma abordagem mais segura é armazenar a senha como um segredo. Por exemplo:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Substitua a senha em *appSettings. JSON* com um espaço reservado. No exemplo a seguir, `{0}` é usado como o espaço reservado para formar um [cadeia de caracteres de formato composto](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Valor do segredo pode ser inserida no espaço reservado para concluir a cadeia de caracteres de conexão:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Lista os segredos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:

```console
dotnet user-secrets list
```

A seguinte saída é exibida:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

No exemplo anterior, dois-pontos em nomes de chave indica que a hierarquia de objetos dentro de *secrets.json*.

## <a name="remove-a-single-secret"></a>Remover um segredo único

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

O aplicativo *secrets.json* arquivo foi modificado para remover o par chave-valor associado a `MoviesConnectionString` chave:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Executando `dotnet user-secrets list` exibirá a seguinte mensagem:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Remover todos os segredos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Execute o seguinte comando do diretório no qual o *. csproj* arquivo existe:

```console
dotnet user-secrets clear
```

Todos os segredos do usuário para o aplicativo tem sido excluídos do *secrets.json* arquivo:

```json
{}
```

Executando `dotnet user-secrets list` exibirá a seguinte mensagem:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Recursos adicionais

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>