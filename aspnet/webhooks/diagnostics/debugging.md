---
uid: webhooks/diagnostics/debugging
title: WebHooks do ASP.NET depuração | Microsoft Docs
author: rick-anderson
description: Como depurar a WebHooks do ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833706"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="7c55e-103">Depuração de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7c55e-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="7c55e-104">Depuração no Azure</span><span class="sxs-lookup"><span data-stu-id="7c55e-104">Debugging in Azure</span></span>

<span data-ttu-id="7c55e-105">Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="7c55e-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="7c55e-106">Com o código-fonte e símbolos de depuração</span><span class="sxs-lookup"><span data-stu-id="7c55e-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="7c55e-107">Além de seu próprio código de depuração, é possível depurar diretamente no Microsoft ASP.NET WebHooks e, de fato todos os .NET.</span><span class="sxs-lookup"><span data-stu-id="7c55e-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="7c55e-108">Isso funciona independentemente de se você depura localmente ou remotamente.</span><span class="sxs-lookup"><span data-stu-id="7c55e-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="7c55e-109">Configurar pela primeira vez, o Visual Studio para localizar o código-fonte e os símbolos acessando **Debug** e, em seguida **opções e configurações**.</span><span class="sxs-lookup"><span data-stu-id="7c55e-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="7c55e-110">Defina as opções como este:</span><span class="sxs-lookup"><span data-stu-id="7c55e-110">Set the options like this:</span></span>

![Opções e configurações](_static/SourceSymbols.png)

<span data-ttu-id="7c55e-112">Em seguida, adicione um link para [symbolsource.org](http://symbolsource.org) para baixar o código-fonte e símbolos.</span><span class="sxs-lookup"><span data-stu-id="7c55e-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="7c55e-113">Vá para o **símbolos** guia do menu acima e adicione o seguinte como um local do símbolo:</span><span class="sxs-lookup"><span data-stu-id="7c55e-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="7c55e-114">Além disso, certifique-se de que o diretório de cache tem um nome curto; Caso contrário, os nomes de arquivo podem obter muito longos que fará com que os símbolos para não carregar.</span><span class="sxs-lookup"><span data-stu-id="7c55e-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="7c55e-115">Um exemplo de caminho é:</span><span class="sxs-lookup"><span data-stu-id="7c55e-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="7c55e-116">As configurações devem ser semelhantes a este:</span><span class="sxs-lookup"><span data-stu-id="7c55e-116">The settings should look similar to this:</span></span>

![Exemplo de local do arquivo de símbolo de opções](_static/SymSource.png)
