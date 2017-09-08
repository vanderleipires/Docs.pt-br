---
title: "Compilação de projetos com Yeoman no núcleo do ASP.NET"
author: spboyer
description: "Este artigo o orienta na criação de uma ASP.NET Core aplicativo web usando o Yeoman gerador de macOS."
keywords: ASP.NET Core, Yeoman, plataforma cruzada, seu aspnet
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: 3a7cd83becc570d2f73014b356977fedb16f29bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Introdução à criação de projetos com Yeoman no núcleo do ASP.NET

[Yeoman](http://yeoman.io/) é um sistema de scaffolding de projeto para a criação de vários tipos de aplicativos. O Yeoman gerador para o ASP.NET Core contém uma variedade de modelos de projeto para iniciar uma nova web, o MVC ou o aplicativo de console.

## <a name="install-nodejs-npm-and-yeoman"></a>Instale o Node. js, npm e Yeoman

### <a name="prerequisites"></a>Pré-requisitos

Node. js e npm são necessários para Yeoman. Baixar do [Node.js](https://nodejs.org/en/). O instalador inclui [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/). Bower também é necessário para instalar estruturas de interface do usuário como a inicialização.

Para instalar Yeoman e Bower, execute o seguinte comando:

```console
npm install -g yo bower
```

>[!Note]
>Se você receber o erro `npm ERR! Please try running this command again as root/Administrator.` no macOS, execute o seguinte comando usando [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Em um prompt de comando, instale o gerador do ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Se você receber um erro de permissão, execute o comando em `sudo` conforme descrito acima.

O `–g` sinalizador instala o gerador de globalmente, para que ele pode ser usado em qualquer caminho.

## <a name="create-an-aspnet-app"></a>Criar um aplicativo ASP.NET

Execute o gerador ASP.NET baseados em Yeoman:

```console
yo aspnet
```

O gerador de exibe um menu. Seta para baixo até o **Web aplicativo básico [sem associação e a autorização]** do projeto e toque em **Enter**:

![Janela de comando: O tipo de aplicativo você deseja criar? Menu de tipos de aplicativos](yeoman/_static/yeoman-yo-aspnet.png)

Selecione inicialização como a estrutura de interface do usuário e toque em **Enter**.

Use "**MyWebApp**" para o nome do aplicativo e, em seguida, toque **Enter**.

Yeoman será scaffold o projeto e seus arquivos de suporte. Próximas etapas sugeridas também são fornecidas na forma de comandos.

![Janela de comando: qual é o nome do seu aplicativo ASP.NET? Prompt de comando](yeoman/_static/yeoman-yo-aspnet-created.png)

O [ASP.NET gerador](https://www.npmjs.com/package/generator-aspnet) cria ASP.NET Core projetos que podem ser carregado no código do Visual Studio, Visual Studio, ou executado na linha de comando.

## <a name="restore-build-and-run"></a>Restaurar, compilar e executar

Execute os comandos sugeridos alterando diretórios para o `MyWebApp` directory. Em seguida, execute `dotnet restore`.

![Janela Comando](yeoman/_static/dotnet-restore.png)

Compilar e executar o aplicativo usando `dotnet build` e `dotnet run`:

![Janela Comando](yeoman/_static/dotnet-build-run.png)

Neste ponto, você pode navegar para a URL mostrada para testar o aplicativo do ASP.NET Core recém-criado.

## <a name="client-side-packages"></a>Pacotes do lado do cliente

Os recursos de front-end são fornecidos pelos modelos a partir de Yeoman usando o gerador de [Bower](xref:client-side/bower) Gerenciador de pacotes do lado do cliente, adicionando *bower. JSON* e *bowerrc* arquivos para restaurar os pacotes do lado do cliente usando o Bower.

O [BundlerMinifier](xref:client-side/bundling-and-minification) componente também é incluído por padrão para facilidade de concatenação (agrupamento) e minimização de CSS, JavaScript e HTML.

## <a name="building-and-running-from-visual-studio"></a>Criação e a execução do Visual Studio

Você pode carregar seu projeto de web do ASP.NET Core gerado diretamente no Visual Studio, em seguida, compilar e executar o projeto a partir daí. Siga as instruções acima Scaffold um novo aplicativo ASP.NET Core usando Yeoman. Desta vez, escolha **aplicativo Web** no menu e o nome do aplicativo `MyWebApp`.

Abra o Visual Studio. No menu Arquivo, selecione Abrir ‣ projeto/solução.

Na caixa de diálogo Abrir projeto, navegue até o *. csproj* de arquivo, selecione-o e clique no **abrir** botão. No Gerenciador de soluções, o projeto deve ser semelhante a captura de tela abaixo.

![Arquivos e pastas de um novo projeto no Gerenciador de soluções](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds um aplicativo da web MVC, compilação do servidor e cliente com ambos suporte. Dependências do lado do servidor são listadas no **dependências para o NuGet** nó e dependências do lado do cliente no **dependências/Bower** nó do Gerenciador de soluções. As dependências são restauradas automaticamente quando o projeto é carregado.

![Sob o nó dependências na exibição de árvore do Gerenciador de soluções, a pasta Bower é abrir lista de suas dependências.](yeoman/_static/yeoman-loading-dependencies.png)

Quando todas as dependências são restauradas, pressione **F5** para executar o projeto. A página inicial padrão exibe no navegador.

![Aplicativo Web aberto no Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Restaurando, criação e hospedagem de uma linha de comando

Você pode preparar e hospedar seu aplicativo web usando a CLI do .NET Core.

Em um prompt de comando, altere o diretório atual para a pasta que contém o projeto (ou seja, a pasta que contém o *. csproj* arquivo):

```console
cd src\MyWebApp
```

Restaure as dependências de pacotes do NuGet do projeto:

```console
dotnet restore
```

Execute o aplicativo:

```console
dotnet run
```

A plataforma cruzada [Kestrel](xref:fundamentals/servers/kestrel) servidor da web será iniciada a escuta na porta 5000.

Abra um navegador da web e navegue até `http://localhost:5000`.

![Aplicativo Web aberto no Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Adicionar ao seu projeto com geradores sub

Usando Yeoman [sub geradores](https://www.github.com/omnisharp/generator-aspnet#sub-generators), você pode adicionar uma `nuget.config` ou um `web.config` depois que o projeto é criado. Por exemplo, execute o seguinte comando do diretório no qual o arquivo deve ser criado:

```console
yo aspnet:nugetconfig
```

O resultado é um arquivo de configuração NuGet chamado `nuget.config` com o seguinte conteúdo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Recursos adicionais

* [Servidores (Kestrel e WebListener)](xref:fundamentals/servers/index)
* [Conceitos básicos](xref:fundamentals/index)
