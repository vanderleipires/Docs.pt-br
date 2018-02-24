---
title: Use o modelo de projeto reagir com retorno
author: SteveSandersonMS
description: "Saiba como começar a usar o modelo de projeto de aplicativo de página única (SPA) do ASP.NET Core para reagir com Redux e criar reagir-aplicativo."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: f52f4a866b8bb2adeee19548df32f3e7a91761d1
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-with-redux-project-template"></a><span data-ttu-id="f5777-103">Use o modelo de projeto reagir com retorno</span><span class="sxs-lookup"><span data-stu-id="f5777-103">Use the React-with-Redux project template</span></span>

> [!NOTE]
> <span data-ttu-id="f5777-104">Esta documentação não está sobre o modelo de projeto reagir com retorno incluída no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="f5777-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="f5777-105">Trata-se o modelo de reagir com retorno mais recente para o qual você pode atualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="f5777-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="f5777-106">O modelo é incluído no ASP.NET Core 2.1 por padrão.</span><span class="sxs-lookup"><span data-stu-id="f5777-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="f5777-107">O modelo de projeto reagir com Redux atualizado fornece um ponto inicial conveniente para aplicativos ASP.NET Core usando reagem, Redux, e [criar reagir-aplicativo](https://github.com/facebookincubator/create-react-app) convenções (CRA) para implementar uma interface de usuário do lado do cliente avançado (IU).</span><span class="sxs-lookup"><span data-stu-id="f5777-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="f5777-108">Com exceção do comando de criação de projeto, todas as informações sobre o modelo reagir com retorno serão o mesmo que o modelo de reagir.</span><span class="sxs-lookup"><span data-stu-id="f5777-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="f5777-109">Para criar esse tipo de projeto, execute `dotnet new reactredux` em vez de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="f5777-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="f5777-110">Para obter mais informações sobre a funcionalidade comum para ambos os modelos baseados em reagir, consulte [reagir a documentação do modelo](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="f5777-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
