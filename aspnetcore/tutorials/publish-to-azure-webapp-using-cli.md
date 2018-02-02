---
title: Publique um aplicativo ASP.NET Core para o Azure usando as ferramentas de linha de comando
author: camsoper
description: "Saiba como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o cliente de linha de comando do Git."
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0418a2695d3afb6dc2c55b8f694a97d62239835f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="61224-103">Implante um aplicativo ASP.NET Core para o Serviço de Aplicativo do Azure da linha de comando</span><span class="sxs-lookup"><span data-stu-id="61224-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="61224-104">Por [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="61224-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="61224-105">Este tutorial mostrará como criar e implantar um aplicativo ASP.NET Core para o Serviço de Aplicativo do Microsoft Azure usando as ferramentas de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="61224-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="61224-106">No fim, você terá um aplicativo Web criado em ASP.NET MVC Core hospedado como um Aplicativo Web do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="61224-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="61224-107">Este tutorial foi criado usando as ferramentas de linha de comando do Windows, mas também pode ser aplicado aos ambientes do macOS e Linux.</span><span class="sxs-lookup"><span data-stu-id="61224-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="61224-108">Neste tutorial, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="61224-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61224-109">Crie um site do Serviço de Aplicativo do Azure usando a CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="61224-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="61224-110">Implante um aplicativo ASP.NET Core para o Serviço de Aplicativo do Azure usando a ferramenta de linha de comando Git</span><span class="sxs-lookup"><span data-stu-id="61224-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61224-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="61224-111">Prerequisites</span></span>

<span data-ttu-id="61224-112">Para concluir este tutorial, você precisará de:</span><span class="sxs-lookup"><span data-stu-id="61224-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="61224-113">Uma [assinatura do Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="61224-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="61224-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="61224-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="61224-115">Cliente de linha de comando [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="61224-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="61224-116">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="61224-116">Create a web application</span></span>

<span data-ttu-id="61224-117">Crie um novo diretório para o aplicativo Web, crie um novo aplicativo MVC do ASP.NET Core e execute o site localmente.</span><span class="sxs-lookup"><span data-stu-id="61224-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="61224-118">Windows</span><span class="sxs-lookup"><span data-stu-id="61224-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="61224-119">Outros</span><span class="sxs-lookup"><span data-stu-id="61224-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Saída da linha de comando](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="61224-121">Teste o aplicativo navegando até http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="61224-121">Test the application by browsing to http://localhost:5000.</span></span>

![O site em execução localmente](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="61224-123">Crie a instância do Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="61224-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="61224-124">Usando o [Azure Cloud Shell](/azure/cloud-shell/quickstart), crie um grupo de recursos, o plano do Serviço de Aplicativo e o aplicativo Web do Serviço de Aplicativo.</span><span class="sxs-lookup"><span data-stu-id="61224-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="61224-125">Antes da implantação, defina as credenciais de implantação a nível de conta usando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="61224-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="61224-126">Uma URL de implantação é necessária para implantar o aplicativo usando o Git.</span><span class="sxs-lookup"><span data-stu-id="61224-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="61224-127">Recupere a URL como esta.</span><span class="sxs-lookup"><span data-stu-id="61224-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="61224-128">Observe a URL exibida terminada em `.git`.</span><span class="sxs-lookup"><span data-stu-id="61224-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="61224-129">Ela é usada na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="61224-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="61224-130">Implantar o aplicativo usando o Git</span><span class="sxs-lookup"><span data-stu-id="61224-130">Deploy the application using Git</span></span>

<span data-ttu-id="61224-131">Você está pronto para implantar a partir da sua máquina local usando o Git.</span><span class="sxs-lookup"><span data-stu-id="61224-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="61224-132">É seguro ignorar quaisquer alertas do Git sobre os términos das linhas.</span><span class="sxs-lookup"><span data-stu-id="61224-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="61224-133">Windows</span><span class="sxs-lookup"><span data-stu-id="61224-133">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="61224-134">Outros</span><span class="sxs-lookup"><span data-stu-id="61224-134">Other</span></span>](#tab/other)
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

<span data-ttu-id="61224-135">O Git solicitará as credenciais de implantação que foram definidas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="61224-135">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="61224-136">Depois de autenticar, o aplicativo será enviado para o local remoto, compilado e implantado.</span><span class="sxs-lookup"><span data-stu-id="61224-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Saída de implantação do Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="61224-138">Testar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="61224-138">Test the application</span></span>

<span data-ttu-id="61224-139">Teste o aplicativo navegando até `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="61224-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="61224-140">Para exibir o endereço no Shell da Nuvem (ou CLI do Azure), use o seguinte:</span><span class="sxs-lookup"><span data-stu-id="61224-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![O aplicativo em execução no Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="61224-142">Limpar</span><span class="sxs-lookup"><span data-stu-id="61224-142">Clean up</span></span>

<span data-ttu-id="61224-143">Quando concluir o teste do aplicativo e a inspeção do código e recursos, exclua o aplicativo Web e plano removendo o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="61224-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="61224-144">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="61224-144">Next steps</span></span>

<span data-ttu-id="61224-145">Neste tutorial, você aprendeu como:</span><span class="sxs-lookup"><span data-stu-id="61224-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="61224-146">Crie um site do Serviço de Aplicativo do Azure usando a CLI do Azure</span><span class="sxs-lookup"><span data-stu-id="61224-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="61224-147">Implante um aplicativo ASP.NET Core para o Serviço de Aplicativo do Azure usando a ferramenta de linha de comando Git</span><span class="sxs-lookup"><span data-stu-id="61224-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="61224-148">A seguir, você aprenderá a usar a linha de comando para implantar um aplicativo Web existente que usa o CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="61224-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="61224-149">Implantar no Azure, na linha de comando com o .NET Core</span><span class="sxs-lookup"><span data-stu-id="61224-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
