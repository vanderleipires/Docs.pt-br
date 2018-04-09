---
title: Use o modelo de projeto reagir com retorno com o ASP.NET Core
author: SteveSandersonMS
description: Saiba como começar a usar o modelo de projeto de aplicativo de página única (SPA) do ASP.NET Core para reagir com Redux e criar reagir-aplicativo.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Use o modelo de projeto reagir com retorno com o ASP.NET Core

> [!NOTE]
> Esta documentação não está sobre o modelo de projeto reagir com retorno incluída no ASP.NET 2.0 de núcleo. Trata-se o modelo de reagir com retorno mais recente para o qual você pode atualizar manualmente. O modelo é incluído no ASP.NET Core 2.1 por padrão.

O modelo de projeto reagir com Redux atualizado fornece um ponto inicial conveniente para aplicativos ASP.NET Core usando reagem, Redux, e [criar reagir-aplicativo](https://github.com/facebookincubator/create-react-app) convenções (CRA) para implementar uma interface de usuário do lado do cliente avançado (IU).

Com exceção do comando de criação de projeto, todas as informações sobre o modelo reagir com retorno serão o mesmo que o modelo de reagir. Para criar esse tipo de projeto, execute `dotnet new reactredux` em vez de `dotnet new react`. Para obter mais informações sobre a funcionalidade comum para ambos os modelos baseados em reagir, consulte [reagir a documentação do modelo](xref:spa/react).
