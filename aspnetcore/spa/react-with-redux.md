---
title: Usar o modelo de projeto do React com Redux com o ASP.NET Core
author: SteveSandersonMS
description: Saiba como começar a trabalhar com o modelo de projeto do SPA (Aplicativo de Página Única) do ASP.NET Core para React com Redux e create-react-app.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 7ec4f6d53a4723ace087b1dc256de7845cb44cc6
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555216"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Usar o modelo de projeto do React com Redux com o ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Esta documentação não é sobre o modelo de projeto do React com Redux incluído no ASP.NET Core 2.0. Ela é sobre o modelo React com Redux mais recente, para o qual você pode atualizar manualmente. O modelo está incluído no ASP.NET Core 2.1 por padrão.

::: moniker-end

O modelo de projeto do React com Redux atualizado fornece um ponto inicial conveniente para aplicativos do ASP.NET Core usando convenções do React, do Redux e de CRA [(create-react-app)](https://github.com/facebookincubator/create-react-app) para implementar uma IU (interface do usuário) avançada do lado do cliente.

Com exceção do comando de criação de projeto, todas as informações sobre o modelo React com Redux são as mesmas que aquelas sobre o modelo React. Para criar esse tipo de projeto, execute `dotnet new reactredux` em vez de `dotnet new react`. Para obter mais informações sobre a funcionalidade comum para ambos os modelos baseados em reagir, confira a [Documentação do modelo React](xref:spa/react).
