---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860934"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="739b2-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="739b2-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="739b2-104">Este documento fornece etapas para criar e executar um aplicativo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="739b2-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="739b2-105">Instale o [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="739b2-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="739b2-106">Crie um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="739b2-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="739b2-107">Abra um shell de comando e insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="739b2-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="739b2-108">Confie no certificado de desenvolvimento HTTPS:</span><span class="sxs-lookup"><span data-stu-id="739b2-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="739b2-109">Windows</span><span class="sxs-lookup"><span data-stu-id="739b2-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="739b2-110">O comando anterior exibe a caixa de diálogo a seguir:</span><span class="sxs-lookup"><span data-stu-id="739b2-110">The preceding command displays the following dialog:</span></span>

  ![Caixa de diálogo de aviso de segurança](_static/cert.png)

  <span data-ttu-id="739b2-112">Selecione **Sim** se você concordar com confiar no certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="739b2-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="739b2-113">macOS</span><span class="sxs-lookup"><span data-stu-id="739b2-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="739b2-114">O comando anterior exibe a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="739b2-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="739b2-115">*Foi solicitada confiança no certificado de desenvolvimento HTTPS. Se o certificado ainda não for confiável, executaremos o seguinte comando:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="739b2-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="739b2-116">\*Esse comando pode solicitar que você insira sua senha para instalar o certificado no conjunto de chaves do sistema.</span><span class="sxs-lookup"><span data-stu-id="739b2-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="739b2-117">Senha:\*</span><span class="sxs-lookup"><span data-stu-id="739b2-117">Password:\*</span></span>

  <span data-ttu-id="739b2-118">Insira sua senha se você concordar em confiar no certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="739b2-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="739b2-119">Linux</span><span class="sxs-lookup"><span data-stu-id="739b2-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="739b2-120">Consulte a documentação para sua distribuição do Linux sobre como confiar no certificado de desenvolvimento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="739b2-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="739b2-121">Execute o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="739b2-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="739b2-122">Procure [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="739b2-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="739b2-123">Clique em **Aceitar** para aceitar a política de privacidade e cookies.</span><span class="sxs-lookup"><span data-stu-id="739b2-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="739b2-124">Este aplicativo não armazena informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="739b2-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="739b2-125">Abra *Pages/About.cshtml* e modifique a página com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="739b2-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="739b2-126">Navegue até [http://localhost:5001/About](http://localhost:5001/About) e verifique as alterações exibidas.</span><span class="sxs-lookup"><span data-stu-id="739b2-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="739b2-127">Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="739b2-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="739b2-128">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="739b2-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="739b2-129">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="739b2-129">Open a command shell.</span></span> <span data-ttu-id="739b2-130">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="739b2-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="739b2-131">Execute o aplicativo com os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="739b2-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="739b2-132">Procure [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="739b2-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="739b2-133">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="739b2-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="739b2-134">O horário no servidor é @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="739b2-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="739b2-135">Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="739b2-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="739b2-136">Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="739b2-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="739b2-137">Crie uma pasta para um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="739b2-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="739b2-138">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="739b2-138">Open a command shell.</span></span> <span data-ttu-id="739b2-139">Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="739b2-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="739b2-140">Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="739b2-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="739b2-141">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="739b2-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="739b2-142">Restaure os pacotes.</span><span class="sxs-lookup"><span data-stu-id="739b2-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="739b2-143">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="739b2-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="739b2-144">O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="739b2-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="739b2-145">Navegue para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="739b2-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
