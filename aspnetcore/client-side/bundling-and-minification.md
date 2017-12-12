---
title: "Empacotamento e minimização no núcleo do ASP.NET"
author: scottaddie
description: "Saiba como otimizar recursos estáticos em um aplicativo ASP.NET Core aplicando técnicas de empacotamento e minimização."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a>Empacotamento e minimização

Por [Scott Addie](https://twitter.com/Scott_Addie)

Este artigo explica os benefícios da aplicação de empacotamento e minimização, incluindo como esses recursos podem ser usados com aplicativos web do ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>O que é o empacotamento e minimização?

Empacotamento e minimização são duas otimizações de desempenho distintos que você pode aplicar em um aplicativo web. Usados juntos, empacotamento e minimização melhoram o desempenho reduzindo o número de solicitações do servidor e reduzindo o tamanho dos ativos estáticos solicitados.

Empacotamento e minimização principalmente melhoram o tempo de carregamento de solicitação de página primeiro. Depois que uma página da web foi solicitada, o navegador armazena em cache os ativos estáticos (JavaScript, CSS e imagens). Consequentemente, empacotamento e minimização não melhoram o desempenho ao solicitar a mesma página ou páginas, no mesmo site que está solicitando os mesmos ativos. Se você não definir o cabeçalho corretamente em seus ativos de expiração e se você não usar o empacotamento e minimização, heurística de atualização do navegador marca os ativos obsoletos depois de alguns dias. Além disso, o navegador requer uma solicitação de validação para cada ativo. Nesse caso, empacotamento e minimização fornecem uma melhoria de desempenho mesmo após a primeira solicitação de página.

### <a name="bundling"></a>Agrupamento

Agrupando combina vários arquivos em um único arquivo. Agrupando reduz o número de solicitações de servidor que são necessários para renderizar um ativo de web, como uma página da web. Você pode criar qualquer número de pacotes individuais especificamente para CSS, JavaScript, etc. Menos arquivos significa menos solicitações HTTP do navegador para o servidor ou do serviço fornecendo o seu aplicativo. Isso resulta em melhor desempenho de carregamento de página primeiro.

### <a name="minification"></a>Minimização

Minimização remove caracteres desnecessárias de código sem alterar a funcionalidade. O resultado é uma redução de tamanho significativo nos ativos solicitados (como CSS, imagens e arquivos JavaScript). Os efeitos colaterais de minimização incluem encurtar nomes de variável para um caractere e remover comentários e espaços em branco desnecessários.

Considere a função JavaScript a seguir:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimização reduz a função para o seguinte:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Além de remover os comentários e espaços em branco desnecessários, os seguintes nomes de parâmetro e variável foram renomeados da seguinte maneira:

Original | Renomeado
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impacto de empacotamento e minimização

A tabela a seguir descreve as diferenças entre carregamento ativos individualmente e usar o empacotamento e minimização:

Ação | Com B/M | Sem B/M | Alteração
--- | :---: | :---: | :---:
Solicitações de arquivo  | 7   | 18     | 157%
KB transferido | 156 | 264.68 | 70%
Tempo de carregamento (ms) | 885 | 2360   | 167%

Navegadores são bastante detalhados em relação a cabeçalhos de solicitação HTTP. O total de bytes enviados métrica viu uma redução significativa ao agrupamento. O tempo de carregamento mostra uma melhoria significativa, no entanto, esse exemplo foi executado localmente. Maiores ganhos de desempenho são obtidos quando usar o empacotamento e minimização com ativos transferido em uma rede.

## <a name="choose-a-bundling-and-minification-strategy"></a>Escolha uma estratégia de empacotamento e minimização

Os modelos de projeto MVC e páginas Razor fornecem uma solução para empacotamento e minimização consiste em um arquivo de configuração JSON. Ferram de terceiros, como o [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) executores de tarefas, realizar as mesmas tarefas com um pouco mais complexidade. Uma ferramenta de terceiros é uma excelente opção quando o fluxo de trabalho de desenvolvimento requer processamento além de empacotamento e minimização&mdash;como otimização linting e imagem. Usando o empacotamento e minimização tempo de design, os arquivos minimizados são criados antes da implantação do aplicativo. Empacotando e minimizando antes da implantação tem a vantagem de carga do servidor reduzido. No entanto, é importante reconhecer que o agrupamento de tempo de design e minimização aumenta a complexidade de compilação e só funciona com arquivos estáticos.

## <a name="configure-bundling-and-minification"></a>Configurar o empacotamento e minimização

Os modelos de projeto MVC e páginas Razor fornecem uma *bundleconfig.json* arquivo de configuração que define as opções para cada pacote. Por padrão, uma configuração de pacote único é definida para o JavaScript personalizado (*wwwroot/js/site.js*) e a folha de estilos (*wwwroot/css/site.css*) arquivos:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Opções do pacote incluem:

* `outputFileName`: O nome do arquivo de pacote de saída. Pode conter um caminho relativo do *bundleconfig.json* arquivo. **Necessário**
* `inputFiles`: Uma matriz de arquivos para agrupar em conjunto. Esses são os caminhos relativos ao arquivo de configuração. **opcional**, * um valor vazio resulta em um arquivo de saída vazia. [Globalização](http://www.tldp.org/LDP/abs/html/globbingref.html) padrões são suportados.
* `minify`: As opções de minimização para o tipo de saída. **opcional**, *padrão:`minify: { enabled: true }`*
  * Opções de configuração estão disponíveis por tipo de arquivo de saída.
    * [Minificador CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minificador de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minificador de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: O sinalizador que indica se é para adicionar arquivos gerados ao arquivo de projeto. **opcional**, *default - false*
* `sourceMap`: O sinalizador que indica se deve gerar um mapa de origem para o arquivo de pacote. **opcional**, *default - false*
* `sourceMapRootPath`: O caminho raiz para armazenar o arquivo de mapa de código-fonte gerado.

## <a name="build-time-execution-of-bundling-and-minification"></a>Execução de tempo de compilação de empacotamento e minimização

O [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacote NuGet permite que a execução de empacotamento e minimização no momento da compilação. O pacote injeta [destinos do MSBuild](/visualstudio/msbuild/msbuild-targets) quais executar compilação e tempo limpo. O *bundleconfig.json* arquivo é analisado pelo processo de compilação para produzir os arquivos de saída com base na configuração de definidos.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Adicionar o *BuildBundlerMinifier* pacote ao seu projeto.

Compile o projeto. A seguir é exibido na janela de saída:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Limpe o projeto. A seguir é exibido na janela de saída:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI do .NET Core](#tab/netcore-cli) 

Adicionar o *BuildBundlerMinifier* pacote ao seu projeto:

```console
dotnet add package BuildBundlerMinifier
```

Se usar o ASP.NET Core 1. x, restaurar o pacote adicionado recentemente:

```console
dotnet restore
```

Compile o projeto:

```console
dotnet build
```

Será exibido o seguinte:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Limpe o projeto:

```console
dotnet clean
```

A seguinte saída é exibida:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Execução ad hoc de empacotamento e minimização

É possível executar as tarefas de empacotamento e minimização em uma base ad hoc, sem criar o projeto. Adicionar o [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacote NuGet ao seu projeto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

Este pacote estende a CLI do núcleo do .NET para incluir o *dotnet pacote* ferramenta. O comando a seguir pode ser executado na janela do Console de Gerenciador de pacote (PMC) ou em um shell de comando:

```console
dotnet bundle
```

> [!IMPORTANT]
> O NuGet Package Manager adiciona as dependências para o arquivo *. csproj como `<PackageReference />` nós. O `dotnet bundle` comando está registrado com o .NET Core CLI somente quando um `<DotNetCliToolReference />` nó é usado. Modifique o arquivo *. csproj adequadamente.

## <a name="add-files-to-workflow"></a>Adicionar arquivos ao fluxo de trabalho

Considere um exemplo no qual adicional *custom.css* arquivo é adicionado a seguir:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Ser minificada *custom.css* e agrupar com *site.css* em uma *site.min.css* de arquivo, adicione o caminho relativo para *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Como alternativa, é possível usar o seguinte padrão de globalização:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Esse padrão de globalização corresponde a todos os arquivos CSS e exclui o padrão de arquivo minimizada.

Compile o aplicativo. Abra *site.min.css* e observe o conteúdo de *custom.css* é acrescentado ao final do arquivo.

## <a name="environment-based-bundling-and-minification"></a>Empacotamento e minimização baseado no ambiente

Como prática recomendada, os arquivos de pacotes e minimizados do seu aplicativo devem ser usados em um ambiente de produção. Durante o desenvolvimento, os arquivos originais fazer para facilitar a depuração do aplicativo.

Especificar quais arquivos a serem incluídos nas suas páginas usando o [auxiliar de marca de ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) em exibições. O auxiliar de marca de ambiente processa apenas seu conteúdo durante a execução no específico [ambientes](xref:fundamentals/environments).

O seguinte `environment` marca processa os arquivos CSS não processados durante a execução no `Development` ambiente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

O seguinte `environment` marca processa os arquivos CSS agrupados e minimizados quando executado em um ambiente diferente de `Development`. Por exemplo, em execução em `Production` ou `Staging` dispara o processamento dessas folhas de estilo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Consumir bundleconfig.json de Gulp

Há casos em que o fluxo de trabalho empacotamento e minimização do aplicativo requer processamento adicional. Exemplos incluem a otimização da imagem, a eliminação de cache e processamento de ativos CDN. Para atender a esses requisitos, que você pode converter o fluxo de trabalho de empacotamento e minimização para usar o Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Usar a extensão de empacotador & Minificador

O Visual Studio [empacotador & Minificador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensão processa a conversão para Gulp.

Clique com botão direito do *bundleconfig.json* no Gerenciador de soluções e selecione **empacotador & Minificador** > **converter Gulp...** :

![Converter para Gulp item de menu de contexto](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

O *gulpfile.js* e *Package. JSON* arquivos são adicionados ao projeto. O suporte [npm](https://www.npmjs.com/) os pacotes listados no *Package. JSON* do arquivo `devDependencies` seção estão instalados.

Execute o seguinte comando na janela de PMC para instalar a CLI Gulp como uma dependência global:

```console
npm i -g gulp-cli
```

O *gulpfile.js* leituras de arquivo do *bundleconfig.json* arquivo para as entradas, saídas e as configurações.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Converter manualmente

Se o Visual Studio e/ou a extensão de empacotador & Minificador não estiverem disponível, converta manualmente.

Adicionar um *Package. JSON* arquivo, com as seguintes `devDependencies`, para a raiz do projeto:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Instalar as dependências, executando o seguinte comando no mesmo nível como *Package. JSON*:

```console
npm i
```

Instale a CLI Gulp como uma dependência global:

```console
npm i -g gulp-cli
```

Copie o *gulpfile.js* arquivo abaixo da raiz do projeto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Executar tarefas de Gulp

Para disparar a tarefa de minimização Gulp antes que o projeto é compilado no Visual Studio, adicione o seguinte [destino do MSBuild](/visualstudio/msbuild/msbuild-targets) para o arquivo *. csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

Neste exemplo, todas as tarefas definidas dentro de `MyPreCompileTarget` destino executar antes de predefinida `Build` destino. Saída semelhante à seguinte é exibida na janela de saída do Visual Studio:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Como alternativa, o Explorador do Executador de tarefas do Visual Studio pode ser usado para associar o Gulp tarefas a eventos específicos do Visual Studio. Consulte [executando tarefas padrão](xref:client-side/using-gulp#running-default-tasks) para obter instruções sobre como fazer isso.

## <a name="additional-resources"></a>Recursos adicionais

* [Usando o Gulp](xref:client-side/using-gulp)
* [Usando o Grunt](xref:client-side/using-grunt)
* [Trabalhando com vários ambientes](xref:fundamentals/environments)
* [Auxiliares de marcação](xref:mvc/views/tag-helpers/intro)
