---
title: "Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no núcleo do ASP.NET"
author: rick-anderson
description: "Mostra como armazenar com segurança os segredos durante o desenvolvimento"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: e112cc5ef9cba5aff6470ce4b9b1091a3c2b2600
ms.sourcegitcommit: f1271b218d7dfdc806ec8f411c81f3750130463d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>Armazenamento seguro de segredos do aplicativo durante o desenvolvimento no núcleo do ASP.NET

<a name=security-app-secrets></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), e [Scott Addie](https://scottaddie.com) 

Este documento mostra como você pode usar a ferramenta Gerenciador de segredo em desenvolvimento para manter os segredos fora do seu código. O ponto mais importante é que você nunca deve armazenar senhas ou outros dados confidenciais no código-fonte, e você não deve usar os segredos de produção no modo de desenvolvimento e teste. Em vez disso, você pode usar o [configuração](../fundamentals/configuration.md) sistema ler esses valores de variáveis de ambiente ou dos valores armazenados usando o Gerenciador de segredo de ferramenta. A ferramenta Gerenciador de segredo ajuda a impedir que os dados confidenciais que está sendo verificado no controle de origem. O [configuração](../fundamentals/configuration.md) sistema pode ler os segredos armazenados com a ferramenta Gerenciador de segredo descrita neste artigo.

A ferramenta Gerenciador de segredo é usada somente em desenvolvimento. Você pode proteger segredos de teste e produção do Azure com o [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) provedor de configuração. Consulte [provedor de configuração do Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) para obter mais informações.

## <a name="environment-variables"></a>Variáveis de ambiente

Para evitar armazenar segredos do aplicativo no código ou em arquivos de configuração local, você pode armazenar segredos em variáveis de ambiente. Você pode configurar o [configuração](../fundamentals/configuration.md) framework para ler valores de variáveis de ambiente chamando `AddEnvironmentVariables`. Você pode usar variáveis de ambiente para substituir valores de configuração para todas as fontes de configuração especificada anteriormente.

Por exemplo, se você criar um novo aplicativo web ASP.NET Core com contas de usuário individuais, ele irá adicionar uma cadeia de caracteres de conexão padrão para o *appSettings. JSON* arquivo no projeto com a chave `DefaultConnection`. A cadeia de caracteres de conexão padrão está configurado para usar LocalDB, que é executado no modo de usuário e não requer uma senha. Quando você implanta seu aplicativo em um servidor de teste ou de produção, você pode substituir o `DefaultConnection` valor de chave com uma configuração de variável de ambiente que contém a cadeia de caracteres de conexão (possivelmente com credenciais confidenciais) para um banco de dados de teste ou de produção servidor.

>[!WARNING]
> Variáveis de ambiente geralmente são armazenadas em texto sem formatação e não são criptografadas. Se o computador ou o processo estiver comprometido, variáveis de ambiente podem ser acessadas por indivíduos não confiáveis. Medidas adicionais para evitar a divulgação de segredos do usuário ainda podem ser necessárias.

## <a name="secret-manager"></a>Gerenciador de segredo

A ferramenta Gerenciador de segredo armazena dados confidenciais para o trabalho de desenvolvimento fora da árvore do projeto. A ferramenta Gerenciador de segredo é uma ferramenta de projeto que pode ser usada para armazenar segredos para um [.NET Core](https://www.microsoft.com/net/core) projeto durante o desenvolvimento. Com a ferramenta Gerenciador de segredo, você pode associar os segredos do aplicativo um projeto específico e compartilhá-los em vários projetos.

>[!WARNING]
> A ferramenta Gerenciador de segredo não criptografar os segredos armazenados e não deve ser tratada como um repositório confiável. Ele destina-se apenas para fins de desenvolvimento. As chaves e valores são armazenados em um arquivo de configuração JSON no diretório de perfil do usuário.

## <a name="installing-the-secret-manager-tool"></a>Instalando a ferramenta Gerenciador de segredo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Clique com botão direito no projeto no Gerenciador de soluções e selecione **editar \<project_name\>. csproj** no menu de contexto. Adicione a linha realçada para o *. csproj* de arquivo e salvar para restaurar o pacote NuGet associado:

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Clique com botão direito no projeto no Gerenciador de soluções novamente e selecione **gerenciar segredos do usuário** no menu de contexto. Esse gesto adiciona um novo `UserSecretsId` nó dentro de um `PropertyGroup` do *. csproj* arquivo, como destacado no exemplo a seguir:

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Salvando o *. csproj* arquivo também abre uma `secrets.json` arquivo no editor de texto. Substitua o conteúdo do `secrets.json` arquivo com o código a seguir:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Adicionar `Microsoft.Extensions.SecretManager.Tools` para o *. csproj* e execute o `dotnet restore`. Você pode usar as mesmas etapas para instalar a ferramenta Gerenciador de segredo usando a linha de comando.

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Teste a ferramenta Gerenciador de segredo executando o seguinte comando:

```console
dotnet user-secrets -h
```

A ferramenta Gerenciador de segredo exibirá uso, opções e a Ajuda do comando.

> [!NOTE]
> Você deve estar no mesmo diretório que o *. csproj* arquivo para executar as ferramentas definidas no *. csproj* do arquivo `DotNetCliToolReference` nós.

A ferramenta Gerenciador de segredo opera em definições de configuração específicas de projeto que são armazenadas no perfil do usuário. Para usar os segredos do usuário, o projeto deve especificar um `UserSecretsId` valor em seu *. csproj* arquivo. O valor de `UserSecretsId` é arbitrária, mas é geralmente exclusivo para o projeto. Normalmente, os desenvolvedores de geram um GUID para o `UserSecretsId`.

Adicionar um `UserSecretsId` para seu projeto no *. csproj* arquivo:

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Use a ferramenta Gerenciador de segredo para definir um segredo. Por exemplo, em uma janela de comando do diretório do projeto, digite o seguinte:

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

Você pode executar a ferramenta Gerenciador de segredo de outros diretórios, mas você deve usar o `--project` opção para passar o caminho para o *. csproj* arquivo:
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Você também pode usar a ferramenta Gerenciador de segredo de lista, remover e limpar os segredos do aplicativo.

-----

## <a name="accessing-user-secrets-via-configuration"></a>Acessando os segredos do usuário por meio da configuração

Acessar o Gerenciador de segredo segredos através do sistema de configuração. Adicionar o `Microsoft.Extensions.Configuration.UserSecrets` empacotar e executar `dotnet restore`.

Adicione a fonte de configuração de segredos do usuário para o `Startup` método:

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Você pode acessar os segredos do usuário via a API de configuração:

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Como funciona a ferramenta Gerenciador de segredo

A ferramenta Gerenciador de segredo abstrai os detalhes de implementação, como onde e como os valores são armazenados. Você pode usar a ferramenta sem conhecer esses detalhes de implementação. Na versão atual, os valores são armazenados em um [JSON](http://json.org/) arquivo de configuração no diretório de perfil do usuário:

* Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

O valor de `userSecretsId` vem do valor especificado em *. csproj* arquivo.

Você não deve gravar o código que depende do local ou o formato dos dados salvos com a ferramenta Gerenciador de segredo, como esses detalhes de implementação podem ser alterado. Por exemplo, os valores secretos são atualmente *não* criptografado hoje, mas pode ser um dia.

## <a name="additional-resources"></a>Recursos adicionais

* [Configuração](../fundamentals/configuration.md)
