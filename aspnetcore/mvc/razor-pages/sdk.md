---
title: SDK do Razor do ASP.NET Core
author: Rick-Anderson
description: Saiba como as Páginas Razor no ASP.NET Core tornam a codificação de cenários centrados em página mais fácil e mais produtiva do que com o uso de MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: cf0e1873c7ce500ce3b8ad2b3367555bdc41a576
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555528"
---
# <a name="aspnet-core-razor-sdk"></a>SDK do Razor do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

O [!INCLUDE [](~/includes/2.1-SDK.md)] inclui o SDK do MSBuild `Microsoft.NET.Sdk.Razor` (SDK do Razor). O SDK do Razor:

* Padroniza a experiência de criação, empacotamento e publicação de projetos que contêm arquivos [Razor](xref:mvc/views/razor) para projetos baseados no ASP.NET Core MVC.
* Inclui um conjunto de destinos, propriedades e itens predefinidos que permitem personalizar a compilação de arquivos Razor.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Usando o SDK do Razor

A maioria dos aplicativos Web não precisam referenciar expressamente o SDK do Razor. 

Para usar o SDK do Razor para criar bibliotecas de classe contendo exibições Razor ou Páginas Razor:

* Use `Microsoft.NET.Sdk.Razor` em vez de `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Normalmente, uma referência de pacote para `Microsoft.AspNetCore.Mvc` é necessária para trazer as dependências adicionais necessárias para criar e compilar Páginas Razor e Exibições Razor. No mínimo, o projeto precisa adicionar referências de pacote para:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Os pacotes anteriores são incluídos em `Microsoft.AspNetCore.Mvc`. A marcação a seguir mostra um arquivo *.csproj* que usa o SDK do Razor para criar arquivos Razor para um aplicativo Páginas Razor do ASP.NET Core:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Propriedades

As seguintes propriedades controlam o comportamento do SDK do Razor como parte de um build de projeto:

* `RazorCompileOnBuild`: Quando `true`, compila e emite o assembly Razor como parte da criação do projeto. Assume o padrão de `true`.
* `RazorCompileOnPublish`: quando `true`, compila e emite o assembly Razor como parte da publicação do projeto. Assume o padrão de `true`.

As propriedades e os itens a seguir são usados para configurar entradas e saídas para o SDK do Razor:

| Itens                                         | Descrição                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Elementos de item (arquivos *.cshtml*) que são entradas para os destinos de geração de código. |
| RazorCompile                                  | Elementos de item (arquivos .cs) que são entradas para os destinos de compilação do Razor. Use este ItemGroup para especificar arquivos adicionais a serem compilados no assembly Razor. |
| RazorTargetAssemblyAttribute                  | Os elementos de item usados para a codificação geram atributos para o assembly Razor. Por exemplo:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Elementos de item adicionados como recursos incorporados ao assembly Razor gerado |

| propriedade                                      | Descrição                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nome do arquivo (sem extensão) do assembly produzido pelo Razor. | 
| RazorOutputPath                               | O diretório de saída do Razor.                                      |
| RazorCompileToolset                           | Usado para determinar o conjunto de ferramentas usado para criar o assembly do Razor. Os valores válidos são `Implicit` e `PrecompilationTool`. |
| EnableDefaultContentItems                     | Quando `true`, inclui a determinados tipos de arquivo, como arquivos *.cshtml*, como conteúdo do projeto. Quando referenciado por meio de Microsoft.NET.Sdk.Web, também inclui todos os arquivos em *wwwroot* e arquivos de configuração.         |
| EnableDefaultRazorGenerateItems               | Quando `true`, inclui arquivos *.cshtml* de itens de `Content` em itens de `RazorGenerate`. |
| GenerateRazorTargetAssemblyInfo               | Quando `true`, gera um arquivo *.cs* que contém atributos especificados pelo `RazorAssemblyAttribute` e os inclui na saída da compilação. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Quando `true`, adiciona um conjunto padrão de atributos de assembly em `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Quando `true`, copia arquivos de itens de RazorGenerate (*.cshtml*) no diretório de publicação. Normalmente, os arquivos Razor não são necessários para um aplicativo publicado quando eles participam da compilação no tempo de build ou no tempo de publicação. Assume o padrão de `false`. |
| CopyRefAssembliesToPublishDirectory            | Quando `true`, copia os itens do assembly de referência no diretório de publicação. Normalmente os assemblies de referência não são necessários para um aplicativo publicado quando a compilação do Razor ocorre no tempo de build ou no tempo de publicação. Definido como `true`, por exemplo, se o aplicativo publicado requer a compilação no tempo de execução, ele modifica os arquivos cshtml no tempo de execução ou usa exibições inseridas. Assume o padrão de `false`. |
| IncludeRazorContentInPack                      | Quando `true`, todos os itens de conteúdo do Razor (arquivos *.cshtml*) serão marcados para inclusão no pacote do NuGet gerado. Assume o padrão de `false`. |
| EmbedRazorGenerateSources | Quando `true`, adiciona itens de RazorGenerate (*.cshtml*) como arquivos incorporados ao assembly Razor gerado. Assume o padrão de `false`. |
| UseRazorBuildServer                           | Quando `true`, usa um processo de servidor de build persistente para descarregar o trabalho de geração de código. Seu valor padrão é `UseSharedCompilation`. |

### <a name="targets"></a>Destinos
O SDK do Razor define dois destinos primários:

* `RazorGenerate` – o código gera arquivos *.cs* dos elementos de item de RazorGenerate. Use a propriedade `RazorGenerateDependsOn` para especificar destinos adicionais que podem ser executados antes ou depois desse destino.
* `RazorCompile` – compila arquivos *.cs* gerados em um assembly Razor. Use `RazorCompileDependsOn` para especificar destinos adicionais que podem ser executados antes ou depois desse destino.
