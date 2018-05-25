---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Auxiliar com páginas da Web ASP.NET do Twitter | Microsoft Docs
author: tfitzmac
description: Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar o auxiliar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Auxiliar do Twitter com páginas da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar os métodos auxiliares.
> 
> Este código para o arquivo Twitter.cshtml foi desenvolvido por **Tian panorâmica** da Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


## <a name="introduction"></a>Introdução

Este tópico demonstra como adicionar um auxiliar do Twitter para o seu aplicativo e usar a sintaxe do Razor para chamar os métodos auxiliares. O auxiliar do Twitter facilita a incorporar botões do Twitter e widgets em seu aplicativo. Para usar um widget do Twitter, como a linha de tempo do usuário ou os resultados da pesquisa para um hashtag, você deve primeiro criar o [widget no Twitter](https://twitter.com/settings/widgets). Depois de criar o widget, você receberá uma identificação de widget. Você pode passar essa id de widget como um parâmetro ao chamar os métodos auxiliares que mostram o widget.

Este tópico foi gravado para a versão 1.1 da API do Twitter. Adicionando diretamente o código auxiliar do Twitter ao seu projeto, você pode atualizar o código auxiliar se altera de API do Twitter.

Para obter informações sobre como instalar o WebMatrix, consulte [Introdução ao ASP.NET páginas da Web 2 - Introdução](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Adicionar auxiliar do Twitter ao seu projeto

Para adicionar o auxiliar do Twitter, primeiro, adicione uma pasta denominada **aplicativo\_código** ao seu projeto. Em seguida, crie um arquivo chamado **Twitter.cshtml**.

![Pasta App_Code](twitter-helper/_static/image1.png)

Substitua o código no Twitter.cshtml com o código a seguir.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chamar os métodos do Twitter de suas páginas da web

O exemplo a seguir mostra como usar os métodos auxiliares do Twitter de uma página em seu projeto. No seu projeto, você desejará substituir os valores de parâmetro com valores que são relevantes para suas necessidades. Você pode usar as ids de widget fornecido para explorar como os métodos funcionam, mas você deve gerar seus próprios widgets para seu projeto.

Nem todos os parâmetros mostrados abaixo são necessários. Os parâmetros opcionais são usados para personalizar como o botão ou o widget é exibido. Por exemplo, o botão siga requer somente o nome de usuário a seguir, mas o exemplo mostra como incluir o número de seguidores e como especificar o tamanho do botão e o idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Ver os resultados

O código acima produz os seguintes botões e widgets. Esses botões e widgets são totalmente funcional, não capturas de tela. Botão a seguir é exibido em espanhol porque o parâmetro de idioma foi definido como **es**.

### <a name="follow-button"></a>Siga o botão

[Execute @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a>Botão Tweet

[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a>Linha do tempo do usuário (perfil)

[TWEETS por @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a>Favoritos

[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a>Lista

[TWEETS de @Microsoft/MS \_consumidor\_faixas](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a>Pesquisar

[TWEETS sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
