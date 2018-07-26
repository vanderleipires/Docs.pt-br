---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228576"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="4cfba-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cfba-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="4cfba-104">Instale o [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="4cfba-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="4cfba-105">Crie um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cfba-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="4cfba-106">Abra um shell de comando e insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="4cfba-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="4cfba-107">Confie no certificado de desenvolvimento HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4cfba-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4cfba-108">Windows</span><span class="sxs-lookup"><span data-stu-id="4cfba-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="4cfba-109">O comando anterior exibe a caixa de diálogo a seguir:</span><span class="sxs-lookup"><span data-stu-id="4cfba-109">The preceding command displays the following dialog:</span></span>

   ![Caixa de diálogo de aviso de segurança](_static/cert.png)

   <span data-ttu-id="4cfba-111">Selecione **Sim** se você concordar com confiar no certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="4cfba-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4cfba-112">macOS</span><span class="sxs-lookup"><span data-stu-id="4cfba-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="4cfba-113">O comando anterior exibe a mensagem a seguir:</span><span class="sxs-lookup"><span data-stu-id="4cfba-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="4cfba-114">*Foi solicitada confiança no certificado de desenvolvimento HTTPS. Se o certificado ainda não for confiável, executaremos o seguinte comando:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Esse comando pode solicitar sua senha para instalar o certificado no conjunto de chaves do sistema.    Senha:*</span><span class="sxs-lookup"><span data-stu-id="4cfba-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="4cfba-115">Insira sua senha se você concordar em confiar no certificado de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="4cfba-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4cfba-116">Linux</span><span class="sxs-lookup"><span data-stu-id="4cfba-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="4cfba-117">Consulte a documentação para sua distribuição do Linux sobre como confiar no certificado de desenvolvimento HTTPS</span><span class="sxs-lookup"><span data-stu-id="4cfba-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="4cfba-118">Execute o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="4cfba-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="4cfba-119">Procure [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="4cfba-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="4cfba-120">Clique em **Aceitar** para aceitar a política de privacidade e cookies.</span><span class="sxs-lookup"><span data-stu-id="4cfba-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="4cfba-121">Este aplicativo não armazena informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="4cfba-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="4cfba-122">Abra *Pages/About.cshtml* e modifique a página com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="4cfba-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="4cfba-123">Navegue até [http://localhost:5001/About](http://localhost:5001/About) e verifique as alterações exibidas.</span><span class="sxs-lookup"><span data-stu-id="4cfba-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="4cfba-124">Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="4cfba-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="4cfba-125">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cfba-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="4cfba-126">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="4cfba-126">Open a command shell.</span></span> <span data-ttu-id="4cfba-127">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="4cfba-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="4cfba-128">Execute o aplicativo com os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="4cfba-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="4cfba-129">Procure [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="4cfba-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="4cfba-130">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="4cfba-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="4cfba-131">O horário no servidor é @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="4cfba-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="4cfba-132">Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="4cfba-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="4cfba-133">Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="4cfba-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="4cfba-134">Crie uma pasta para um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cfba-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="4cfba-135">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="4cfba-135">Open a command shell.</span></span> <span data-ttu-id="4cfba-136">Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="4cfba-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="4cfba-137">Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="4cfba-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="4cfba-138">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4cfba-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="4cfba-139">Restaure os pacotes.</span><span class="sxs-lookup"><span data-stu-id="4cfba-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="4cfba-140">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4cfba-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="4cfba-141">O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="4cfba-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="4cfba-142">Navegue para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4cfba-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
