---
uid: webhooks/diagnostics/debugging
title: "ASP.NET WebHooks depuração | Microsoft Docs"
author: rick-anderson
description: Como depurar WebHooks ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="11455-103">ASP.NET WebHooks depuração</span><span class="sxs-lookup"><span data-stu-id="11455-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="11455-104">Depuração no Azure</span><span class="sxs-lookup"><span data-stu-id="11455-104">Debugging in Azure</span></span>

<span data-ttu-id="11455-105">Para depurar seu aplicativo Web durante a execução no Azure, consulte o tutorial [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="11455-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="11455-106">Depuração com símbolos e fonte</span><span class="sxs-lookup"><span data-stu-id="11455-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="11455-107">Além de depuração de seu próprio código, é possível depurar diretamente no Microsoft ASP.NET WebHooks e, na verdade todos do .NET.</span><span class="sxs-lookup"><span data-stu-id="11455-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="11455-108">Isso funciona independentemente de se você depurar localmente ou remotamente.</span><span class="sxs-lookup"><span data-stu-id="11455-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="11455-109">Configurar pela primeira vez, o Visual Studio para localizar a origem e símbolos acessando **depurar** e **opções e configurações**.</span><span class="sxs-lookup"><span data-stu-id="11455-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="11455-110">Defina as opções como este:</span><span class="sxs-lookup"><span data-stu-id="11455-110">Set the options like this:</span></span>

![Opções e configurações](_static/SourceSymbols.png)

<span data-ttu-id="11455-112">Em seguida, adicionar um link para [symbolsource.org](http://symbolsource.org) para baixar o código-fonte e símbolos.</span><span class="sxs-lookup"><span data-stu-id="11455-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="11455-113">Vá para o **símbolos** guia do menu acima e adicione o seguinte como um local de símbolo:</span><span class="sxs-lookup"><span data-stu-id="11455-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="11455-114">Além disso, certifique-se de que o diretório de cache tem um nome curto; Caso contrário, os nomes de arquivo podem obter muito que fará com que os símbolos não carregar.</span><span class="sxs-lookup"><span data-stu-id="11455-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="11455-115">Um exemplo de caminho é:</span><span class="sxs-lookup"><span data-stu-id="11455-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="11455-116">As configurações devem ser semelhantes a este:</span><span class="sxs-lookup"><span data-stu-id="11455-116">The settings should look similar to this:</span></span>

![Exemplo de local de arquivo de símbolo de opções](_static/SymSource.png)
