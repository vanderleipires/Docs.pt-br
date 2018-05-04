---
title: Compilação e pré-compilação da exibição do Razor no ASP.NET Core
author: rick-anderson
description: Saiba como habilitar a compilação e a pré-compilação da exibição do MVC Razor nos aplicativos ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Compilação e pré-compilação da exibição do Razor no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

As exibições do Razor são compiladas em tempo de execução quando a exibição é invocada. O ASP.NET Core 1.1.0 e superior pode opcionalmente compilar exibições do Razor e implantá-las com o aplicativo – um processo conhecido como pré-compilação. Os modelos de projeto do ASP.NET Core 2.x habilitam a pré-compilação por padrão.

> [!IMPORTANT]
> A pré-compilação da exibição do Razor não está disponível no momento durante a execução de uma [SCD (implantação autossuficiente)](/dotnet/core/deploying/#self-contained-deployments-scd) no ASP.NET Core 2.0. O recurso estará disponível para SCDs na versão 2.1. Para obter mais informações, consulte [A compilação de exibição falha durante a compilação cruzada para Linux no Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Considerações sobre a pré-compilação:

* As exibições de pré-compilação resultam em um menor pacote publicado e um tempo de inicialização mais rápido.
* Não é possível editar arquivos do Razor depois que de pré-compilar exibições. As exibições editadas não estarão presentes no pacote publicado. 

Para implantar exibições pré-compiladas:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Se o projeto for direcionado ao .NET Framework, inclua uma referência de pacote a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se o projeto for direcionado ao .NET Core, nenhuma alteração será necessária.

Os modelos de projeto do ASP.NET Core 2.x definem `MvcRazorCompileOnPublish` de forma implícita como `true` por padrão, o que significa que este nó pode ser removido com segurança do arquivo *.csproj*. Se você preferir que isso seja explícito, não há nenhum problema em configurar a propriedade `MvcRazorCompileOnPublish` como `true`. A seguinte amostra *.csproj* realça essa configuração:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Defina `MvcRazorCompileOnPublish` como `true` e inclua uma referência de pacote a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. A seguinte amostra *.csproj* realça essas configurações:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Prepare o aplicativo para uma [implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) com o [comando de publicação da CLI do .NET Core](/dotnet/core/tools/dotnet-publish). Por exemplo, execute o seguinte comando na raiz do projeto:

```console
dotnet publish -c Release
```

Um arquivo *<nome_do_projeto>.PrecompiledViews.dll*, que contém as exibições do Razor compiladas, é produzido quando a pré-compilação é bem-sucedida. Por exemplo, a captura de tela abaixo mostra o conteúdo de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Exibições do Razor dentro da DLL](view-compilation/_static/razor-views-in-dll.png)
