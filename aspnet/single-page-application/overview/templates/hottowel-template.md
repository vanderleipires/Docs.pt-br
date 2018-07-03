---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de hot Towel | Microsoft Docs
author: madskristensen
description: Modelo de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388527"
---
<a name="hot-towel-template"></a>Modelo hot Towel
====================
por [Mads Kristensen](https://github.com/madskristensen)

> O modelo de MVC Hot Towel é escrito por John Papa
> 
> Escolha a versão para baixar:
> 
> [Modelo do MVC hot Towel para Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modelo do MVC hot Towel para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: Porque você não deseja ir para o SPA sem um!


Deseja criar um SPA, mas não souber por onde começar? Use o Hot Towel e em segundos você terá um SPA e todas as ferramentas que você precisa para compilar nele!

Hot Towel cria um ótimo ponto de partida para a criação de um aplicativo de página única (SPA) com o ASP.NET. Fora da caixa você ele fornece uma estrutura modular para seu código, navegação de modo de exibição, vinculação de dados, gerenciamento de dados avançados e estilo simple, porém elegante. Hot Towel fornece tudo o que você precisa para criar um SPA, portanto, você pode se concentrar em seu aplicativo, não os detalhes técnicos.

> Saiba mais sobre como criar um SPA partir [John Papa vídeos, tutoriais e cursos da Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Estrutura de aplicativo

SPA de hot Towel fornece uma pasta do aplicativo que contém os arquivos JavaScript e HTML que definem o seu aplicativo.

Dentro da pasta de aplicativo:

- durandal
- serviços
- ViewModels
- modos de exibição

A pasta de aplicativo contém uma coleção de módulos. Esses módulos encapsulam a funcionalidade e declarar dependências nos outros módulos. A pasta de modos de exibição contém o HTML para seu aplicativo e a pasta viewmodels contém a lógica de apresentação para os modos de exibição (um padrão MVVM comum). A pasta de serviços é ideal para hospedar todos os serviços comuns que seu aplicativo pode precisar, como recuperação de dados HTTP ou interação com o armazenamento local. É comum para vários viewmodels usar novamente o código dos módulos de serviço.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estrutura de aplicativo do lado de servidor de ASP.NET MVC

Hot Towel baseia-se a estrutura MVC do ASP.NET familiar e avançada.

- Aplicativo\_iniciar
- Conteúdo
- Controladores
- Modelos
- Scripts
- Exibições

## <a name="featured-libraries"></a>Bibliotecas em destaque

- ASP.NET MVC
- API da Web do ASP.NET
- Otimização da Web do ASP.NET - agrupamento e minificação
- [Breeze](http://Breezejs.com) -gerenciamento de dados avançados
- [Durandal](http://Durandaljs.com) -navegação e a exibição de composição
- [Knockout. js](http://Knockoutjs.com) -associações de dados
- [Require](http://requirejs.org) -modularidade com AMD e otimização
- [Toastr.js](http://jpapa.me/c7toastr) -mensagens pop-up
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) - estilo de CSS robusta

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalando por meio do modelo SPA do Visual Studio 2012 Hot Towel

Hot Towel pode ser instalado como um modelo do Visual Studio 2012. Basta clicar `File`  |  `New Project` e escolha `ASP.NET MVC 4 Web Application`. Em seguida, selecione o ' Hot Towel aplicativos de uma página "modelo e execução!

## <a name="installing-via-the-nuget-package"></a>Instalando por meio do pacote NuGet

Hot Towel também é um pacote do NuGet que incrementa um projeto ASP.NET MVC vazio existente. Instalar apenas usando o Nuget e, em seguida, executar!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Como criar no Hot Towel?

Basta começar a adicionar código!

1. Adicionar seu próprio código do lado do servidor, preferencialmente o Entity Framework e API da Web (o que realmente se destacam com a Breeze)
2. Adicionar modos de exibição para o `App/views` pasta
3. Adicionar viewmodels para o `App/viewmodels` pasta
4. Adicionar associações de dados HTML e o Knockout novos modos de exibição
5. Atualizar as rotas de navegação em `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Instruções passo a passo do HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml é a rota inicial e o modo de exibição para o aplicativo MVC. Ele contém todas as metamarcas padrão, links de css e referências de JavaScript que você esperaria. O corpo contém um único `<div>` que é onde todo o conteúdo (seus modos de exibição) será colocado quando eles são solicitados. O `@Scripts.Render` usa Require para executar o ponto de entrada para o código do aplicativo, que está contido no `main.js` arquivo. Uma tela inicial é fornecida para demonstrar como criar uma tela inicial, enquanto o aplicativo é carregado.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

O `main.js` arquivo contém o código que será executado assim que seu aplicativo é carregado. Isso é onde você deseja define suas rotas de navegação, defina seu início os modos de exibição e executar qualquer programa de instalação/inicialização como uma desobstrução os dados do aplicativo.

O `main.js` arquivo define vários dos módulos do durandal para ajudar a iniciar o aplicativo Iniciar. A instrução definir ajuda a resolver as dependências de módulos para que fiquem disponíveis para a função. Primeiro as mensagens de depuração estiverem habilitadas, que enviam mensagens sobre quais eventos que o aplicativo está executando a janela do console. O código App.Start.&lt;2} informa a estrutura de durandal para iniciar o aplicativo. As convenções são definidas para que o durandal sabe todas as exibições e viewmodels estão contidos nas mesmas pastas nomeadas, respectivamente. Por fim, o `app.setRoot` é acionada carrega o `shell` usando um modelo predefinido `entrance` animação.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Exibições

Modos de exibição são encontrados no `App/views` pasta.

### <a name="shellhtml"></a>shell.html

O `shell.html` contém o layout mestre para seu HTML. Todos os outros modos de exibição serão compostos em algum lugar no lado do seu `shell` modo de exibição. Hot Towel fornece um `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé. Cada uma dessas regiões é carregada com conteúdo forma a outros modos de exibição quando solicitado.

O `compose` associações para o cabeçalho e rodapé são codificados em Hot Towel para apontar para o `nav` e `footer` exibe, respectivamente. A associação de composição para a seção `#content` está associado ao `router` o item do Active Directory do módulo. Em outras palavras, quando você clica em um link de navegação é o modo de exibição correspondente é carregado nessa área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

O `nav.html` contém os links de navegação para o SPA. Isso é onde a estrutura do menu pode ser colocada, por exemplo. Geralmente isso é dados vinculados (usando o Knockout) para o `router` módulo para exibir a navegação que você definiu no `shell.js`. O Knockout procura a vinculação de dados, atributos e associa-os para o `shell` viewmodel para exibir as rotas de navegação e mostrar uma barra de progresso (usando o Twitter Bootstrap) se o `router` módulo está no meio de navegação de um modo de exibição para outra (consulte `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. HTML e details.html

Essas exibições contêm HTML para exibições personalizadas. Quando o `home` link na `nav` menu do modo de exibição é clicado, o `home` modo de exibição será colocado na área de conteúdo do `shell` modo de exibição. Esses modos de exibição podem ser aumentados ou substituídos por suas próprias exibições personalizadas.

### <a name="footerhtml"></a>footer.html

O `footer.html` contém o HTML que aparece no rodapé, na parte inferior do `shell` modo de exibição.

## <a name="viewmodels"></a>ViewModels

ViewModels são encontrados no `App/viewmodels` pasta.

### <a name="shelljs"></a>Shell.js

O `shell` viewmodel contém propriedades e funções que estão associadas ao `shell` modo de exibição. Geralmente, isso é onde as associações de navegação de menu são encontradas (consulte a `router.mapNav` lógica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home. js e details.js

Esses viewmodels contêm as propriedades e funções que estão associadas ao `home` modo de exibição. Ele também contém a lógica de apresentação para o modo de exibição e é a cola entre os dados e o modo de exibição.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Serviços

Serviços são localizados na pasta/serviços de aplicativos. O ideal é que os serviços futuros, como um módulo dataservice, que é responsável por obter e postar dados remotos, poderia ser colocados.

### <a name="loggerjs"></a>logger.js

Hot Towel fornece um `logger` módulo na pasta de serviços. O `logger` módulo é ideal para as mensagens de log para o console e ao usuário no pop-up de notificações do sistema.
