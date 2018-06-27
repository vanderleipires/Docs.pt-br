---
title: Publicar um aplicativo ASP.NET Core no Azure com as ferramentas de linha de comando
author: camsoper
description: Saiba como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o cliente de linha de comando do Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279240"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publicar um aplicativo ASP.NET Core no Azure com as ferramentas de linha de comando

Por [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Este tutorial mostrará como criar e implantar um aplicativo ASP.NET Core para o Serviço de Aplicativo do Microsoft Azure usando as ferramentas de linha de comando. No fim, você terá um aplicativo Web Razor Pages compilado em ASP.NET Core hospedado como um Aplicativo Web do Serviço de Aplicativo do Azure. Este tutorial foi criado usando as ferramentas de linha de comando do Windows, mas também pode ser aplicado aos ambientes do macOS e Linux.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Crie um site do Serviço de Aplicativo do Azure usando a CLI do Azure
> * Implante um aplicativo ASP.NET Core para o Serviço de Aplicativo do Azure usando a ferramenta de linha de comando Git

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisará de:

* Uma [assinatura do Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* Cliente de linha de comando [Git](https://www.git-scm.com/)

## <a name="create-a-web-app"></a>Como criar um aplicativo Web

Crie um novo diretório para o aplicativo Web, crie um novo aplicativo Razor Pages ASP.NET Core e, em seguida, execute o site localmente.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[Outros](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Saída da linha de comando](publish-to-azure-webapp-using-cli/_static/new_prj.png)

Teste o aplicativo navegando até `http://localhost:5000`.

![O site em execução localmente](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>Crie a instância do Serviço de Aplicativo do Azure

Usando o [Azure Cloud Shell](/azure/cloud-shell/quickstart), crie um grupo de recursos, o plano do Serviço de Aplicativo e o aplicativo Web do Serviço de Aplicativo.

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

Antes da implantação, defina as credenciais de implantação a nível de conta usando o seguinte comando:

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Uma URL de implantação é necessária para implantar o aplicativo usando Git. Recupere a URL como esta.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

Observe a URL exibida terminada em `.git`. Ela é usada na próxima etapa.

## <a name="deploy-the-app-using-git"></a>Implantar o aplicativo usando Git

Você está pronto para implantar a partir da sua máquina local usando o Git.

> [!NOTE]
> É seguro ignorar quaisquer alertas do Git sobre os términos das linhas.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[Outros](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

O Git solicitará as credenciais de implantação que foram definidas anteriormente. Depois de autenticar, o aplicativo será enviado para o local remoto, compilado e implantado.

![Saída de implantação do Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>Testar o aplicativo

Teste o aplicativo navegando até `https://<web app name>.azurewebsites.net`. Para exibir o endereço no Shell da Nuvem (ou CLI do Azure), use o seguinte:

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![O aplicativo em execução no Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Limpar

Quando concluir o teste do aplicativo e a inspeção do código e recursos, exclua o aplicativo Web e plano removendo o grupo de recursos.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Crie um site do Serviço de Aplicativo do Azure usando a CLI do Azure
> * Implante um aplicativo ASP.NET Core para o Serviço de Aplicativo do Azure usando a ferramenta de linha de comando Git

A seguir, você aprenderá a usar a linha de comando para implantar um aplicativo Web existente que usa o CosmosDB.

> [!div class="nextstepaction"]
> [Implantar no Azure, na linha de comando com o .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
