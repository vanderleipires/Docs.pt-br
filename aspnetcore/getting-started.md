---
title: Introdução ao ASP.NET Core
author: rick-anderson
description: Um tutorial rápido que cria e executa um aplicativo simples Olá, Mundo usando o ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="cac03-103">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cac03-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="cac03-104">Instale o [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="cac03-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="cac03-105">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cac03-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="cac03-106">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="cac03-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="cac03-107">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="cac03-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="cac03-108">Insira o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="cac03-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="cac03-109">Execute o aplicativo com os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="cac03-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="cac03-110">Procure [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="cac03-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="cac03-111">Abra *Pages/About.cshtml* e modifique a página para exibir a mensagem "Olá, mundo!</span><span class="sxs-lookup"><span data-stu-id="cac03-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="cac03-112">O horário no servidor é @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="cac03-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="cac03-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="cac03-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="cac03-114">Navegue até [ http://localhost:5000/About ](http://localhost:5000/About) e verifique as alterações.</span><span class="sxs-lookup"><span data-stu-id="cac03-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="cac03-115">Instale o **Instalador do SDK** do .NET Core do SDK 1.0.4 que se encontra na [página Todos os downloads do .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="cac03-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="cac03-116">Crie uma pasta para um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cac03-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="cac03-117">No macOS e no Linux, abra uma janela de terminal.</span><span class="sxs-lookup"><span data-stu-id="cac03-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="cac03-118">No Windows, abra um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="cac03-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="cac03-119">Se você instalou uma versão posterior do SDK no computador, crie um arquivo *global.json* para selecionar o SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="cac03-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="cac03-120">Crie um novo projeto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cac03-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="cac03-121">Restaure os pacotes.</span><span class="sxs-lookup"><span data-stu-id="cac03-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="cac03-122">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cac03-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="cac03-123">O comando [dotnet run](/dotnet/core/tools/dotnet-run) cria o aplicativo primeiro, se necessário.</span><span class="sxs-lookup"><span data-stu-id="cac03-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="cac03-124">Navegue para `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cac03-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end