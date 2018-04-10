---
title: Usar os modelos de Aplicativo de Página Única com o ASP.NET Core
author: SteveSandersonMS
description: Saiba como instalar e começar a trabalhar com os modelos de projeto do SPA (Aplicativo de Página Única) ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Usar os modelos de Aplicativo de Página Única com o ASP.NET Core

> [!NOTE]
> O SDK do .NET Core 2.0.x inclui modelos de projeto mais antigos para Angular, React e React with Redux. Esta documentação não é sobre esses modelos de projeto antigos. Essa documentação é para os modelos Angular, React e React with Redux mais recentes, que podem ser instalados manualmente no ASP.NET Core 2.0. Os modelos são incluídos por padrão com o ASP.NET Core 2.1.

## <a name="prerequisites"></a>Pré-requisitos

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), versão 6 ou posterior

## <a name="installation"></a>Instalação

Se você tiver o ASP.NET Core 2.0, execute o seguinte comando para instalar os modelos do ASP.NET Core atualizados para Angular, React e React with Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Usar os modelos

- [Usar o modelo de projeto Angular](xref:spa/angular)
- [Usar o modelo de projeto React](xref:spa/react)
- [Usar o modelo de projeto React with Redux](xref:spa/react-with-redux)
