---
title: "Empacotamento e minimização no núcleo do ASP.NET"
author: spboyer
description: 
keywords: "ASP.NET Core, empacotamento e minimização, CSS, JavaScript, Minificada, BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: d8512bdd49b61019f22a49900bdd65086d821a6b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>Empacotamento e minimização no núcleo do ASP.NET

Empacotamento e minimização são duas técnicas você pode usar para melhorar o desempenho de carregamento de página para seu aplicativo da web em ASP.NET. Agrupando combina vários arquivos em um único arquivo. Minimização executa várias otimizações de código diferente para scripts e CSS, o que resulta em cargas menores. Usados juntos, empacotamento e minimização melhora o desempenho de tempo de carregamento, reduzindo o número de solicitações para o servidor e reduzindo o tamanho dos ativos solicitados (como arquivos CSS e JavaScript).

Este artigo explica os benefícios de usar o empacotamento e minimização, incluindo como esses recursos podem ser usados com aplicativos do ASP.NET Core.

## <a name="overview"></a>Visão Geral

Em aplicativos do ASP.NET Core, há várias opções para Empacotando e minimizando os recursos do lado do cliente. Os modelos de núcleo para MVC fornecem uma solução de fora da caixa usando um arquivo de configuração e o pacote BuildBundlerMinifier NuGet. Ferramentas de terceiros, como [Gulp](using-gulp.md) e [Grunt](using-grunt.md) também estão disponíveis para executar as mesmas tarefas devem exigir seus processos de fluxo de trabalho adicional ou complexidades. Usando o empacotamento e minimização tempo de design, os arquivos minimizados são criados antes da implantação do aplicativo. Empacotando e minimizando antes da implantação tem a vantagem de carga do servidor reduzido. No entanto, é importante reconhecer que o agrupamento de tempo de design e minimização aumenta a complexidade de compilação e só funciona com arquivos estáticos.

Empacotamento e minimização principalmente melhoram o tempo de carregamento de solicitação de página primeiro. Depois que uma página da web foi solicitada, o navegador armazena em cache os ativos (JavaScript, CSS e imagens) para o empacotamento e minimização não fornecerá qualquer aumento de desempenho ao solicitar a mesma página ou páginas no mesmo site solicitando os mesmo ativos. Se você não definir o expira cabeçalho corretamente em seus ativos e você não usar o empacotamento e minimização, heurística de atualização do navegador marcará os ativos obsoletos depois de alguns dias e o navegador exigirá uma solicitação de validação para cada ativo. Nesse caso, empacotamento e minimização fornecem um aumento de desempenho mesmo após a primeira solicitação de página.

### <a name="bundling"></a>Agrupamento

Agrupamento é um recurso que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Porque o agrupamento combina vários arquivos em um único arquivo, ele reduz o número de solicitações para o servidor que são necessárias para recuperar e exibir um ativo de web, como uma página da web. Você pode criar outros pacotes, JavaScript e CSS. Menos arquivos significa menos solicitações HTTP do seu navegador para o servidor ou do serviço que fornece seu aplicativo. Isso resulta em melhor desempenho de carregamento de página primeiro.

### <a name="minification"></a>Minimização

Minimização executa várias otimizações de código diferente para reduzir o tamanho dos ativos solicitados (como CSS, imagens, arquivos JavaScript). Resultados comuns de minimização incluem removendo comentários e espaços em branco desnecessários e encurtar nomes de variável para um caractere.

Considere a função JavaScript a seguir:

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

Depois de minimização, a função será reduzida para o seguinte:

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

Além de remover os comentários e espaços em branco desnecessários, os nomes de variáveis e parâmetros a seguir foram renomeados (reduzido) da seguinte maneira:

Original | Renomeado
--- | :---:
imageTagAndImageID | t
imageContext | a
imageElement | R

## <a name="impact-of-bundling-and-minification"></a>Impacto de empacotamento e minimização

A tabela a seguir mostra várias diferenças importantes entre listando todos os ativos individualmente e usar o empacotamento e minimização em uma página da web simples:

Ação | Com B/M | Sem B/M | Alteração
--- | :---: | :---: | :---:
Solicitações de arquivo |7 | 18 | 157%
KB transferido | 156 | 264.68 | 70%
Tempo de carregamento (MS) | 885 | 2360 | 167%

Os bytes enviados tinham uma redução significativa com agrupamento como navegadores são bastante detalhados com os cabeçalhos HTTP que se aplicam em solicitações. O tempo de carregamento mostra uma grande melhoria, no entanto, esse exemplo foi executado localmente. Você obterá maiores ganhos de desempenho ao usar o empacotamento e minimização com ativos transferido em uma rede.

## <a name="using-bundling-and-minification-in-a-project"></a>Usando o empacotamento e minimização em um projeto

O modelo de projeto MVC fornece uma `bundleconfig.json` arquivo de configuração que define as opções para cada pacote. Por padrão, uma configuração de pacote único é definida para o JavaScript personalizado (`wwwroot/js/site.js`) e a folha de estilos (`wwwroot/css/site.css`) arquivos.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

Opções do pacote incluem:

* outputFileName - nome do arquivo de pacote de saída. Pode conter um caminho relativo do `bundleconfig.json` arquivo. **Necessário**
* inputFiles - a matriz de arquivos para agrupar em conjunto. Esses são os caminhos relativos ao arquivo de configuração. **opcional**, * um valor vazio resulta em um arquivo de saída vazia. [Globalização](http://www.tldp.org/LDP/abs/html/globbingref.html) padrões são suportados.
* minificada - opções de minimização para a saída de tipo. **opcional**, *padrão:`minify: { enabled: true }`*
  * Opções de configuração estão disponíveis por tipo de arquivo de saída.
    * [Minificador CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minificador de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [Minificador de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* Incluirnoproj - adicionar arquivos gerados ao arquivo de projeto. **opcional**, *default - false*
* sourceMaps - gerar mapas de origem para o arquivo de pacote. **opcional**, *default - false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

Abra `bundleconfig.json` no Visual Studio, se seu ambiente não tem a extensão instalada; um prompt é apresentado sugerindo que haja um que pode ajudá-lo a esse tipo de arquivo.

![Sugestão de extensão BuildBundlerMinifier](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

Exibir extensões de selecionar e instalar o **empacotador & Minificador** extensão (reiniciar requer o Visual Studio).

![Sugestão de extensão BuildBundlerMinifier](../client-side/bundling-and-minification/_static/view-extension.png)

Quando a reinicialização estiver concluída, você precisa configurar a compilação para executar os processos de minimizar e agrupando os ativos do lado do cliente. Clique com botão direito do `bundleconfig.json` de arquivo e selecione *habilitar pacote na compilação... *.

Compilar o projeto e o `bundleconfig.json` está incluído no processo de compilação para produzir os arquivos de saída com base na configuração.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Código do Visual Studio ou a linha de comando

O processo de empacotamento e minimização usando gestos de GUI; de unidade do Visual Studio e a extensão No entanto, os mesmos recursos estão disponíveis com o `dotnet` pacote CLI e BuildBundlerMinifier NuGet.

Adicione o pacote NuGet ao seu projeto:

```console
dotnet add package BuildBundlerMinifier
```

Restaure as dependências:

```console
dotnet restore
```

Compile o aplicativo:

```console
dotnet build
```

A saída do comando de compilação mostra os resultados da minimização e/ou agrupamento de acordo com o que está configurado.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>Adicionando arquivos

Neste exemplo, um arquivo CSS adicional é adicionado chamado `custom.css` e configurado para o empacotamento e minimização com `site.css`, resultando em um único `site.min.css`.

Custom.CSS

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

Adicionar o caminho relativo para `bundleconfig.json`.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> Como alternativa, pode ser usado o padrão de globalização - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` que obtém CSS todos os arquivos e exclui o padrão de arquivo minimizada.

Compilar o aplicativo e se você abrir `site.min.css`, agora você vai notar que o conteúdo de `custom.css` foi acrescentado ao final do arquivo.

## <a name="controlling-bundling-and-minification"></a>Controlando o empacotamento e minimização

Em geral, você deseja usar os arquivos de pacotes e minimizados do aplicativo somente em um ambiente de produção. Durante o desenvolvimento, você deseja usar os arquivos originais, seu aplicativo mais fácil de depurar.

Você pode especificar quais scripts e arquivos CSS para incluir em suas páginas usando o auxiliar de marca de ambiente em suas páginas de layout (consulte [auxiliares de marcação](../mvc/views/tag-helpers/index.md)). O auxiliar de marca de ambiente renderizará somente seu conteúdo durante a execução em ambientes específicos. Consulte [trabalhando com vários ambientes](../fundamentals/environments.md) para obter detalhes sobre como especificar o ambiente atual.

A seguinte marca de ambiente processará os arquivos CSS não processados durante a execução no `Development` ambiente:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

Esta marca de ambiente processará os arquivos CSS agrupados e minimizados somente durante a execução no `Production` ou `Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Consumindo bundleconfig.json de Gulp

Se o fluxo de trabalho de empacotamento e minimização de aplicativo exige processos adicionais, como o processamento de imagem, a eliminação de cache, processamento de assest CDN, etc., você pode converter o processo de pacote e Minify em Gulp.

> [!NOTE]
> Opção de conversão disponível apenas no Visual Studio 2015 e 2017.

Clique com botão direito do `bundleconfig.json` e selecione **converter Gulp... **. Isso irá gerar o `gulpfile.js` e instalar os pacotes necessários npm.

![Converter em Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

O `gulpfile.js` produzido leituras de `bundleconfig.json` arquivo para a configuração, portanto, ele pode continuar a ser usado para as entradas/saídas e as configurações.

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Para habilitar o Gulp quando o projeto é compilado no Visual Studio de 2017, adicione o seguinte para o arquivo *. csproj:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Para habilitar o Gulp quando o projeto é compilado no Visual Studio 2015, adicione o seguinte para o `project.json` arquivo:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>Recursos adicionais

* [Usando o Gulp](using-gulp.md)
* [Usando o Grunt](using-grunt.md)
* [Trabalhando com vários ambientes](../fundamentals/environments.md)
* [Auxiliares de Marcas](../mvc/views/tag-helpers/index.md)
