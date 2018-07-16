---
title: Compilação e pré-compilação do arquivo do Razor no ASP.NET Core
author: rick-anderson
description: Saiba mais sobre os benefícios de pré-compilação arquivos do Razor e como realizar pré-compilação desses arquivos em um aplicativo do ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 9355d467ca819ea8c6292963b31367ad5ca36d55
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938531"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Compilação de arquivo do Razor no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
Um arquivo do Razor é compilado em tempo de execução, quando o modo de exibição do MVC associado é chamado. Não há suporte para a publicação de arquivos do Razor de tempo de build. Como opção, os arquivos do Razor podem ser compilados durante a publicação e implantados com o aplicativo &mdash; usando a ferramenta de pré-compilação.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
Um arquivo do Razor é compilado em tempo de execução, quando o modo de exibição do MVC ou da Página do Razor associada é chamado. Não há suporte para a publicação de arquivos do Razor de tempo de build. Como opção, os arquivos do Razor podem ser compilados durante a publicação e implantados com o aplicativo &mdash; usando a ferramenta de pré-compilação.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Um arquivo do Razor é compilado em tempo de execução, quando o modo de exibição do MVC ou da Página do Razor associada é chamado. Os arquivos do Razor são compilados em tempo de build e de publicação usando o [SDK do Razor](xref:razor-pages/sdk).
::: moniker-end

## <a name="precompilation-considerations"></a>Considerações sobre a pré-compilação

Estes são os efeitos colaterais da pré-compilação de arquivos do Razor:

* Pacote publicado menor
* Tempo de inicialização mais rápido
* Não é possível editar arquivos do Razor&mdash;o conteúdo associado está ausente do pacote publicado.

## <a name="deploy-precompiled-files"></a>Implantar arquivos pré-compilados

::: moniker range=">= aspnetcore-2.1"
A compilação em tempo de build e de publicação de arquivos do Razor está habilitada por padrão pelo SDK do Razor. Há suporte para edição de arquivos do Razor depois que eles são atualizados em tempo de build. Por padrão, nenhum arquivo *.cshtml* é implantado com o aplicativo; somente a *Views.dll* compilada.

> [!IMPORTANT]
> O SDK do Razor é eficaz somente quando não há propriedades específicas de pré-compilação definidas no arquivo de projeto. Por exemplo, definir a propriedade `MvcRazorCompileOnPublish` do arquivo *.csproj* como `true` desabilita o SDK do Razor.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Se o projeto for direcionado ao .NET Framework, instale o pacote NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Se o projeto for direcionado ao .NET Core, nenhuma alteração será necessária.

Os modelos de projeto do ASP.NET Core 2.x definem implicitamente a propriedade `MvcRazorCompileOnPublish` como `true` por padrão. Consequentemente, esse elemento pode ser removido com segurança do arquivo *.csproj*.

> [!IMPORTANT]
> A pré-compilação do arquivo do Razor não está disponível durante a execução de uma [SCD (implantação autossuficiente)](/dotnet/core/deploying/#self-contained-deployments-scd) no ASP.NET Core 2.0.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Defina a propriedade `MvcRazorCompileOnPublish` como `true` e instale o pacote NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). A seguinte amostra *.csproj* realça essas configurações:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Prepare o aplicativo para uma [implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) com o [comando de publicação da CLI do .NET Core](/dotnet/core/tools/dotnet-publish). Por exemplo, execute o seguinte comando na raiz do projeto:

```console
dotnet publish -c Release
```

Um arquivo *<nome_do_projeto>.PrecompiledViews.dll*, que contém os arquivos do Razor compilados, é produzido quando a pré-compilação é bem-sucedida. Por exemplo, a captura de tela abaixo mostra o conteúdo de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:

![Exibições do Razor dentro da DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Recursos adicionais

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end
