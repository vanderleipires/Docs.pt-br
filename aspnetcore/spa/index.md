---
title: Usar os modelos de Aplicativo de Página Única com o ASP.NET Core
author: SteveSandersonMS
description: Saiba como instalar e começar a trabalhar com os modelos de projeto do SPA (Aplicativo de Página Única) ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555554"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="0adaa-103">Usar os modelos de Aplicativo de Página Única com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0adaa-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="0adaa-104">O SDK do .NET Core 2.0.x inclui modelos de projeto mais antigos para Angular, React e React with Redux.</span><span class="sxs-lookup"><span data-stu-id="0adaa-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="0adaa-105">Esta documentação não é sobre esses modelos de projeto antigos.</span><span class="sxs-lookup"><span data-stu-id="0adaa-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="0adaa-106">Essa documentação é para os modelos Angular, React e React with Redux mais recentes, que podem ser instalados manualmente no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0adaa-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="0adaa-107">Os modelos são incluídos por padrão com o ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0adaa-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="0adaa-108">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0adaa-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="0adaa-109">[Node.js](https://nodejs.org), versão 6 ou posterior</span><span class="sxs-lookup"><span data-stu-id="0adaa-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="0adaa-110">Instalação</span><span class="sxs-lookup"><span data-stu-id="0adaa-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0adaa-111">Os modelos já estão instalados com o ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0adaa-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0adaa-112">Se você tiver o ASP.NET Core 2.0, execute o seguinte comando para instalar os modelos do ASP.NET Core atualizados para Angular, React e React with Redux:</span><span class="sxs-lookup"><span data-stu-id="0adaa-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="0adaa-113">Usar os modelos</span><span class="sxs-lookup"><span data-stu-id="0adaa-113">Use the templates</span></span>

* [<span data-ttu-id="0adaa-114">Usar o modelo de projeto Angular</span><span class="sxs-lookup"><span data-stu-id="0adaa-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="0adaa-115">Usar o modelo de projeto React</span><span class="sxs-lookup"><span data-stu-id="0adaa-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="0adaa-116">Usar o modelo de projeto React with Redux</span><span class="sxs-lookup"><span data-stu-id="0adaa-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
