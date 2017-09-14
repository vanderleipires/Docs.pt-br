---
title: Migrando do ASP.NET Core 1.x para 2.0
author: scottaddie
description: "Este artigo descreve os pré-requisitos e as etapas mais comuns para a migração de um projeto ASP.NET Core 1.x para o ASP.NET Core 2.0."
keywords: ASP.NET Core, migrando
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>Migrando do ASP.NET Core 1.x para o ASP.NET Core 2.0

Por [Scott Addie](https://github.com/scottaddie)

Neste artigo, vamos orientá-lo pela atualização de um projeto existente ASP.NET Core 1.x para o ASP.NET Core 2.0. A migração do aplicativo para o ASP.NET Core 2.0 permite que você aproveite [muitos novos recursos e melhorias de desempenho](https://go.microsoft.com/fwlink/?linkid=854094). 

Os aplicativos ASP.NET Core 1.x existentes baseiam-se em modelos de projeto específicos à versão. Conforme a estrutura do ASP.NET Core evolui, os modelos do projeto e o código inicial contido neles também. Além de atualizar a estrutura ASP.NET Core, você precisa atualizar o código para o aplicativo.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Pré-requisitos
Consulte [Introdução ao ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Atualizar TFM (Moniker da Estrutura de Destino)
Projetos direcionados ao .NET Core devem usar o [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de uma versão maior ou igual ao .NET Core 2.0. Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `netcoreapp2.0`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]

Projetos direcionados ao .NET Framework devem usar o TFM de uma versão maior ou igual ao .NET Framework 4.6.1. Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `net461`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> O .NET Core 2.0 oferece uma área de superfície muito maior do que o .NET Core 1.x. Se você estiver direcionando o .NET Framework exclusivamente devido a APIs ausentes no .NET Core 1.x, o direcionamento do .NET Core 2.0 provavelmente funcionará.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Atualizar a versão do SDK do .NET Core em global.json
Se a solução depender de um arquivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para direcionar uma versão específica do SDK do .NET Core, atualize sua propriedade `version` para que ela use a versão 2.0 instalada no computador:

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Referências do pacote de atualização
O arquivo *.csproj* em um projeto 1.x lista cada pacote NuGet usado pelo projeto.

Em um projeto ASP.NET Core 2.0 direcionado ao .NET Core 2.0, uma única referência de [metapacote](xref:fundamentals/metapackage) no arquivo *.csproj* substitui a coleção de pacotes:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]

Todos os recursos do ASP.NET Core 2.0 e do Entity Framework Core 2.0 são incluídos no metapacote.

Os projetos do ASP.NET Core 2.0 direcionados ao .NET Framework devem continuar a referenciar pacotes NuGet individuais. Atualize o atributo `Version` de cada nó `<PackageReference />` para 2.0.0.

Por exemplo, esta é a lista de nós `<PackageReference />` usados em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Framework:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Atualizar ferramentas da CLI do .NET Core
No arquivo *.csproj*, atualize o atributo `Version` de cada nó `<DotNetCliToolReference />` para 2.0.0.

Por exemplo, esta é a lista de ferramentas da CLI usadas em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Core 2.0:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Renomear a propriedade de Fallback de Destino do Pacote
O arquivo *.csproj* de um projeto 1.x usou um nó `PackageTargetFallback` e uma variável:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]

Renomeie o nó e a variável como `AssetTargetFallback`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Atualizar o método Main em Program.cs
Em projetos 1.x, o método `Main` de *Program.cs* era parecido com este:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

Em projetos 2.0, o método `Main` de *Program.cs* foi simplificado:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]

A adoção deste novo padrão do 2.0 é altamente recomendado e é necessário para que os recursos de produto como as [Migrações do Entity Framework Core](xref:data/ef-mvc/migrations) funcionem. Por exemplo, a execução de `Update-Database` na janela do Console do Gerenciador de Pacotes ou de `dotnet ef database update` na linha de comando (em projetos convertidos no ASP.NET Core 2.0) gera o seguinte erro:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>Examinar a configuração Compilação de Exibição do Razor
Um tempo de inicialização mais rápido do aplicativo e pacotes publicados menores são de extrema importância para você. Por esses motivos, a opção [Compilação de exibição do Razor](xref:mvc/views/view-compilation) é habilitada por padrão no ASP.NET Core 2.0.

A configuração da propriedade `MvcRazorCompileOnPublish` como verdadeiro não é mais necessária. A menos que você esteja desabilitando a compilação de exibição, a propriedade poderá ser removida do arquivo *.csproj*.

Ao direcionar o .NET Framework, você ainda precisa referenciar explicitamente o pacote NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) no arquivo *.csproj*:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Depender dos recursos “Light-Up” do Application Insights
A instalação fácil da instrumentação de desempenho do aplicativo é importante. Agora, você pode contar com os novos recursos “light-up” do [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponíveis nas ferramentas do Visual Studio 2017.

Os projetos do ASP.NET Core 1.1 criados no Visual Studio 2017 adicionaram o Application Insights por padrão. Se não estiver usando o SDK do Application Insights diretamente, fora de *Program.cs* e *Startup.cs*, siga estas etapas:

1. Remova o seguinte nó `<PackageReference />` do arquivo *.csproj*:
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]

2. Remova a invocação do método de extensão `UseApplicationInsights` de *Program.cs*:

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Remova a chamada à API do lado do cliente do Application Insights de *_Layout.cshtml*. Ela inclui as duas seguintes linhas de código:

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]

Se você estiver usando o SDK do Application Insights diretamente, continue fazendo isso. O [metapacote](xref:fundamentals/metapackage) do 2.0 inclui a última versão do Application Insights. Portanto, um erro de downgrade do pacote será exibido se você estiver referenciando uma versão mais antiga.

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>Adotar melhorias de autenticação/identidade
O ASP.NET Core 2.0 tem um novo modelo de autenticação e uma série de alterações significativas no ASP.NET Core Identity. Se você criou o projeto com a opção Contas de Usuário Individuais habilitada ou se adicionou manualmente a autenticação ou a identidade, consulte [Migrando a autenticação e a identidade para o ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Recursos adicionais
- [Últimas alterações no ASP.NET Core 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
