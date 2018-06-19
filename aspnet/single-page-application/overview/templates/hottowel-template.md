---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de toalhas hot | Microsoft Docs
author: madskristensen
description: Modelo de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875013"
---
<a name="hot-towel-template"></a>Modelo de toalhas ativo
====================
por [Mads Kristensen](https://github.com/madskristensen)

> O modelo de MVC toalhas ativa é escrito por John Papa
> 
> Escolha a versão para download:
> 
> [Modelo MVC toalhas ativa para o Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modelo MVC toalhas ativa para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot toalhas: Porque você não deseja ir para o SPA sem um!


Deseja criar um SPA, mas não é possível decidir onde iniciar? Usar um toalhas ativa e em segundos você terá um SPA e todas as ferramentas que necessárias para criar nele!

Toalhas hot cria um excelente ponto de partida para a criação de um aplicativo de página única (SPA) com o ASP.NET. Sem a necessidade de você ele fornece uma estrutura modular para o código, modo de exibição de navegação, associação de dados, gerenciamento de dados avançado e estilo simple, mas elegante. Toalhas hot fornece todo o que necessário para criar um SPA, para que você possa se concentrar em seu aplicativo, não o direcionamento.

> Saiba mais sobre como criar um SPA de [John Papa vídeos, tutoriais e Pluralsight cursos](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Estrutura de aplicativo

Hot SPA de toalhas fornece uma pasta de aplicativo que contém os arquivos JavaScript e HTML que definem o aplicativo.

Dentro da pasta do aplicativo:

- Durandal
- serviços
- ViewModels
- modos de exibição

A pasta de aplicativo contém uma coleção de módulos. Esses módulos encapsulam funcionalidades e declarar dependências em outros módulos. Modos de exibição contém o HTML para seu aplicativo e a pasta de viewmodels contém a lógica de apresentação para os modos de exibição (um padrão comum de MVVM). A pasta de serviços é ideal para acomodar todos os serviços comuns que talvez seja necessário seu aplicativo, como recuperação de dados HTTP ou interação com o armazenamento local. É comum para várias viewmodels reutilizar o código dos módulos de serviço.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estrutura de aplicativo do ASP.NET MVC Server lado

Toalhas hot cria a estrutura ASP.NET MVC familiares e avançados.

- Aplicativo\_iniciar
- Conteúdo
- Controladores
- Modelos
- Scripts
- Exibições

## <a name="featured-libraries"></a>Bibliotecas em destaque

- ASP.NET MVC
- API da Web do ASP.NET
- Otimização da Web do ASP.NET - empacotamento e minimização
- [Breeze.js](http://Breezejs.com) -gerenciamento de dados avançados
- [Durandal.js](http://Durandaljs.com) -navegação e a composição de exibição
- [Knockout. js](http://Knockoutjs.com) -associações de dados
- [Require.js](http://requirejs.org) -modularidade com AMD e de otimização
- [Toastr.js](http://jpapa.me/c7toastr) -mensagens pop-up
- [Inicialização do Twitter](http://twitter.github.com/bootstrap/) - robustas folhas de estilos

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalando por meio do modelo do Visual Studio 2012 toalhas Hot SPA

Toalhas ativa podem ser instalada como um modelo do Visual Studio 2012. Basta clicar em `File`  |  `New Project` e escolha `ASP.NET MVC 4 Web Application`. Selecione o ' aplicativo de página única toalhas Hot "modelo e execução!

## <a name="installing-via-the-nuget-package"></a>Instalando por meio do pacote do NuGet

Toalhas hot também é um pacote do NuGet que aumenta um projeto ASP.NET MVC vazio existente. Instalar apenas usando o Nuget e, em seguida, execute!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Como criar em um toalhas ativa?

Basta começar a adicionar código!

1. Adicionar seu próprio código do lado do servidor, preferencialmente o Entity Framework e WebAPI (o que realmente se destacam com Breeze.js)
2. Adicionar modos de exibição para o `App/views` pasta
3. Adicionar viewmodels para o `App/viewmodels` pasta
4. Adicionar associações de dados HTML e Knockout a novos modos de exibição
5. Atualizar as rotas de navegação `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Instruções passo a passo do HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

cshtml é a rota inicial e o modo de exibição para o aplicativo MVC. Ele contém todas as marcas meta padrão, links de css e você esperaria de referências de JavaScript. O corpo contém um único `<div>` que é onde todo o conteúdo (seus modos de exibição) será colocado quando forem solicitados. O `@Scripts.Render` usa Require.js para executar o ponto de entrada para o código do aplicativo, que está contido no `main.js` arquivo. Uma tela inicial é fornecida para demonstrar como criar uma tela inicial, enquanto seu aplicativo carrega.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

O `main.js` arquivo contém o código que será executado assim que seu aplicativo é carregado. Isso é onde você deseja definir seu rotas de navegação, defina seu início os modos de exibição e executar qualquer instalação/inicialização como desobstrução dados do aplicativo.

O `main.js` arquivo define vários dos módulos do durandal para iniciar o aplicativo Iniciar. A instrução definir ajuda a resolver as dependências de módulos para que fiquem disponíveis para a função. Primeiro as mensagens de depuração estão habilitadas, quais enviam mensagens sobre os eventos que está executando o aplicativo para a janela do console. O código de App informa durandal framework para iniciar o aplicativo. As convenções são definidas para que durandal sabe todas as exibições e viewmodels contidos nas mesmas pastas nomeadas, respectivamente. Por fim, o `app.setRoot` inicia, carrega o `shell` usando um modelo predefinido `entrance` animação.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Exibições

Modos de exibição são encontrados no `App/views` pasta.

### <a name="shellhtml"></a>shell.html

O `shell.html` contém o layout mestre para HTML. Todos os outros modos de exibição serão compostos em algum lugar no lado do seu `shell` exibição. Toalhas hot fornece um `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé. Cada uma dessas regiões é carregada com conteúdo formam a outros modos de exibição quando solicitado.

O `compose` associações para o cabeçalho e rodapé são codificados em um toalhas ativa para apontar para o `nav` e `footer` exibições, respectivamente. A associação de redação para a seção `#content` está associada ao `router` item ativo do módulo. Em outras palavras, quando você clica em um link de navegação é o modo de exibição correspondente é carregado nessa área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

O `nav.html` contém os links de navegação para o SPA. Isso é onde a estrutura de menu pode ser colocada, por exemplo. Geralmente é associados (usando Knockout) de dados para o `router` módulo para exibir a navegação definido no `shell.js`. Knockout procura a associação de dados de atributos e associa-os para o `shell` viewmodel para exibir as rotas de navegação e mostrar uma progressbar (usando a inicialização do Twitter) se o `router` módulo está no meio de navegação de um modo de exibição para outra (consulte `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>o arquivo Home.HTML e details.html

Essas exibições contêm HTML para exibições personalizadas. Quando o `home` link no `nav` menu do modo de exibição é clicado, o `home` exibição será colocada na área de conteúdo do `shell` exibição. Esses modos de exibição podem ser aumentados ou substituídos por suas próprias exibições personalizadas.

### <a name="footerhtml"></a>footer.html

O `footer.html` contém HTML que aparece no rodapé, na parte inferior do `shell` exibição.

## <a name="viewmodels"></a>ViewModels

ViewModels são encontrados no `App/viewmodels` pasta.

### <a name="shelljs"></a>Shell.js

O `shell` viewmodel contém as propriedades e funções que estão associadas ao `shell` exibição. Geralmente, isso é onde as associações de navegação do menu são encontradas (consulte o `router.mapNav` lógica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js e details.js

Esses viewmodels contêm as propriedades e as funções que estão associadas ao `home` exibição. Ele também contém a lógica de apresentação para o modo de exibição e é a união entre os dados e o modo de exibição.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Serviços

Serviços são localizados na pasta/serviços de aplicativos. Idealmente, os serviços futuros como um módulo dataservice, que é responsável por obter e postar dados remotos, podem ser colocados.

### <a name="loggerjs"></a>logger.js

Toalhas hot fornece um `logger` módulo na pasta serviços. O `logger` módulo é ideal para as mensagens de log para o console e o usuário no pop-up notificações do sistema.
