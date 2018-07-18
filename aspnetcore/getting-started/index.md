---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216207"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="ebe22-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebe22-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="ebe22-104">Instale o [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="ebe22-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="ebe22-105">Crie um projeto do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebe22-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="ebe22-106">Abra um shell de comando e insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ebe22-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="ebe22-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="ebe22-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="ebe22-108">Confie no certificado de desenvolvimento HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ebe22-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ebe22-109">Windows</span><span class="sxs-lookup"><span data-stu-id="ebe22-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="ebe22-110">macOS</span><span class="sxs-lookup"><span data-stu-id="ebe22-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="ebe22-111">Linux</span><span class="sxs-lookup"><span data-stu-id="ebe22-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="ebe22-112">Execute o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="ebe22-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="ebe22-113">Procure [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="ebe22-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="ebe22-114">Clique em **Aceitar** para aceitar a política de privacidade e cookies.</span><span class="sxs-lookup"><span data-stu-id="ebe22-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="ebe22-115">Este aplicativo não armazena informações pessoais.</span><span class="sxs-lookup"><span data-stu-id="ebe22-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="ebe22-116">Abra *Pages/About.cshtml* e modifique a página com a seguinte marcação realçada:</span><span class="sxs-lookup"><span data-stu-id="ebe22-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="ebe22-117">Navegue até [http://localhost:5001/About](http://localhost:5001/About) e verifique as alterações exibidas.</span><span class="sxs-lookup"><span data-stu-id="ebe22-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="ebe22-118">Instale o [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="ebe22-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="ebe22-119">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebe22-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="ebe22-120">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="ebe22-120">Open a command shell.</span></span> <span data-ttu-id="ebe22-121">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ebe22-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="ebe22-122">Execute o aplicativo com os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ebe22-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="ebe22-123">Procure [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="ebe22-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="ebe22-124">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="ebe22-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="ebe22-125">O horário no servidor é @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="ebe22-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="ebe22-126">Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="ebe22-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="ebe22-127">Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="ebe22-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="ebe22-128">Crie uma pasta para um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebe22-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="ebe22-129">Abra um shell de comando.</span><span class="sxs-lookup"><span data-stu-id="ebe22-129">Open a command shell.</span></span> <span data-ttu-id="ebe22-130">Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ebe22-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="ebe22-131">Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="ebe22-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="ebe22-132">Crie um novo projeto ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebe22-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="ebe22-133">Restaure os pacotes.</span><span class="sxs-lookup"><span data-stu-id="ebe22-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="ebe22-134">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ebe22-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="ebe22-135">O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="ebe22-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="ebe22-136">Navegue para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="ebe22-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
