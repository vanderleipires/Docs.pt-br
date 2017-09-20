---
title: "Pré-compilação e a compilação de exibição do razor"
author: rick-anderson
description: "Um documento de referência que explicam como habilitar a compilação de exibição Razor do MVC e pré-compilação em aplicativos do ASP.NET Core."
keywords: "ASP.NET Core, compilação de exibição Razor, Razor pré-compilação, pré-compilação do Razor"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilação de exibição Razor e pré-compilação no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Modos de exibição Razor são compilados em tempo de execução quando o modo de exibição é invocado. ASP.NET Core 1.1.0 e superior pode opcionalmente compilar exibições Razor e implantá-las com o aplicativo &mdash; um processo conhecido como pré-compilação. Os modelos de projeto do ASP.NET Core 2. x habilitam pré-compilação por padrão.

> [!NOTE]
> A pré-compilação de exibição Razor não está disponível ao fazer uma [Self-Contained implantação](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) em versões do ASP.NET Core 2.0.0 e versões anteriores.

Considerações de pré-compilação:

* Exibições de pré-compilação resulta em uma menor pacote publicado e o tempo de inicialização mais rápido.
* Depois que você pré-compilar modos de exibição, você não pode editar arquivos do Razor. Os modos de exibição editados não está presentes no pacote publicado. 

Para implantar pré-compilado exibições:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se seu projeto se destina ao .NET Framework, incluir uma referência de pacote para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se seu projeto se destina ao .NET Core, não são necessárias alterações.

Os modelos de projeto do ASP.NET Core 2. x definem implicitamente `MvcRazorCompileOnPublish` para `true` por padrão, que significa que este nó pode ser removido com segurança do *. csproj* arquivo. Se você preferir ser explícita, não há qualquer problema na configuração de `MvcRazorCompileOnPublish` propriedade `true`. O seguinte *. csproj* exemplo destaca essa configuração:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Definir `MvcRazorCompileOnPublish` para `true`e incluir uma referência de pacote para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. O seguinte *. csproj* exemplo realça essas configurações:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
