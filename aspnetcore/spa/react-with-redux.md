---
title: Use o modelo de projeto reagir com retorno
author: SteveSandersonMS
description: "Saiba como começar a usar o modelo de projeto do ASP.NET Core única página aplicativo (SPA) release candidate para reagir com Redux e criar reagir-aplicativo."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: c92aa070a307129afd91c432739625b5e0f8b29e
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/05/2018
---
# <a name="use-the-react-with-redux-project-template-release-candidate"></a><span data-ttu-id="cd0b6-103">Use o modelo de projeto reagir com retorno (versão release candidate)</span><span class="sxs-lookup"><span data-stu-id="cd0b6-103">Use the React-with-Redux project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="cd0b6-104">Esta documentação não é sobre o modelo de projeto reagir com retorno lançado.</span><span class="sxs-lookup"><span data-stu-id="cd0b6-104">This documentation is not about the released React-with-Redux project template.</span></span> <span data-ttu-id="cd0b6-105">**Esta documentação é sobre a versão release candidate do modelo reagir com retorno.**</span><span class="sxs-lookup"><span data-stu-id="cd0b6-105">**This documentation is about the release candidate of the React-with-Redux template.**</span></span> <span data-ttu-id="cd0b6-106">Esperamos que acompanham a versão de lançamento antecipado 2018.</span><span class="sxs-lookup"><span data-stu-id="cd0b6-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="cd0b6-107">O modelo de projeto reagir com Redux atualizado fornece um ponto inicial conveniente para aplicativos ASP.NET Core usando reagem, Redux, e [criar reagir-aplicativo](https://github.com/facebookincubator/create-react-app) convenções (CRA) para implementar uma interface de usuário do lado do cliente avançado (IU).</span><span class="sxs-lookup"><span data-stu-id="cd0b6-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="cd0b6-108">Com exceção do comando de criação de projeto, todas as informações sobre o modelo reagir com retorno serão o mesmo que o modelo de reagir.</span><span class="sxs-lookup"><span data-stu-id="cd0b6-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="cd0b6-109">Para criar esse tipo de projeto, execute `dotnet new reactredux` em vez de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="cd0b6-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="cd0b6-110">Para obter mais informações sobre a funcionalidade comum para ambos os modelos baseados em reagir, consulte [reagir a documentação do modelo](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="cd0b6-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
