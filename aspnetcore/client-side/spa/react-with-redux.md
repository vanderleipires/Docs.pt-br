---
title: Usar o modelo de projeto do React com Redux com o ASP.NET Core
author: SteveSandersonMS
description: Saiba como começar a trabalhar com o modelo de projeto do SPA (Aplicativo de Página Única) do ASP.NET Core para React com Redux e create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291479"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="ae9bb-103">Usar o modelo de projeto do React com Redux com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae9bb-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="ae9bb-104">Esta documentação não é sobre o modelo de projeto do React com Redux incluído no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="ae9bb-105">Ela é sobre o modelo React com Redux mais recente, para o qual você pode atualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="ae9bb-106">O modelo está incluído no ASP.NET Core 2.1 por padrão.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="ae9bb-107">O modelo de projeto do React com Redux atualizado fornece um ponto inicial conveniente para aplicativos do ASP.NET Core usando convenções do React, do Redux e de CRA [(create-react-app)](https://github.com/facebookincubator/create-react-app) para implementar uma IU (interface do usuário) avançada do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="ae9bb-108">Com exceção do comando de criação de projeto, todas as informações sobre o modelo React com Redux são as mesmas que aquelas sobre o modelo React.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="ae9bb-109">Para criar esse tipo de projeto, execute `dotnet new reactredux` em vez de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="ae9bb-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="ae9bb-110">Para obter mais informações sobre a funcionalidade comum para ambos os modelos baseados em reagir, confira a [Documentação do modelo React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="ae9bb-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
