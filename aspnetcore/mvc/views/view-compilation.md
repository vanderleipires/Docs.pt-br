---
title: "Compilação e pré-compilação da exibição do Razor"
author: rick-anderson
description: "Um documento de referência que explica como habilitar a compilação e pré-compilação da exibição do Razor MVC em aplicativos ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se o projeto for direcionado ao .NET Framework, inclua uma referência de pacote a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Se o projeto for direcionado ao .NET Core, nenhuma alteração será necessária.

Os modelos de projeto do ASP.NET Core 2.x definem `MvcRazorCompileOnPublish` de forma implícita como `true` por padrão, o que significa que este nó pode ser removido com segurança do arquivo *.csproj*. Se você preferir que isso seja explícito, não há nenhum problema em configurar a propriedade `MvcRazorCompileOnPublish` como `true`. A seguinte amostra *.csproj* realça essa configuração:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Defina `MvcRazorCompileOnPublish` como `true` e inclua uma referência de pacote a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. A seguinte amostra *.csproj* realça essas configurações:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Prepare o aplicativo para uma [implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) executando um comando como o seguinte na raiz do projeto:

```console
dotnet publish -c Release
```

Um arquivo *<nome_do_projeto>.PrecompiledViews.dll*, que contém as exibições do Razor compiladas, é produzido quando a pré-compilação é bem-sucedida. Por exemplo, a captura de tela abaixo mostra o conteúdo de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Exibições do Razor dentro da DLL](view-compilation/_static/razor-views-in-dll.png)
