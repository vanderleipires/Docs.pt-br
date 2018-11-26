---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Auxiliar do Twitter com páginas da Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto o WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar o auxiliar de...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299424"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Auxiliar do Twitter com páginas da Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Twitter auxiliares estão obsoletos. Para ferramentas de envolvimento de mais recente do Twitter, para sites, consulte [do Twitter para visão geral de sites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Este tópico e o aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto o WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar os métodos auxiliares.
> 
> Esse código para o arquivo Twitter.cshtml foi desenvolvido pela **Tian panorâmica** da Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="introduction"></a>Introdução

Este tópico demonstra como adicionar um auxiliar do Twitter ao seu aplicativo e usar a sintaxe do Razor para chamar os métodos auxiliares. O auxiliar do Twitter facilita a incorporar botões do Twitter e widgets em seu aplicativo. Para usar um widget do Twitter, como o cronograma de um usuário ou os resultados da pesquisa para uma hashtag, você deve primeiro criar o [widget no Twitter](https://twitter.com/settings/widgets). Depois de criar seu widget, você receberá uma id de widget. Você pode passar essa id de widget como um parâmetro ao chamar os métodos auxiliares que mostram o widget.

Este tópico foi escrito para a versão 1.1 da API do Twitter. Ao adicionar o código auxiliar do Twitter diretamente ao seu projeto, você pode atualizar o código auxiliar se altera a API do Twitter.

Para obter informações sobre como instalar o WebMatrix, consulte [apresentando o ASP.NET Web Pages 2 - Introdução ao](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Adicionar o auxiliar do Twitter ao seu projeto

Para adicionar o auxiliar do Twitter, em primeiro lugar, adicione uma pasta denominada **App\_código** ao seu projeto. Em seguida, crie um arquivo chamado **Twitter.cshtml**.

![Pasta App_Code](twitter-helper/_static/image1.png)

Substitua o código padrão no Twitter.cshtml com o código a seguir.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chamar métodos do Twitter de suas páginas da web

O exemplo a seguir mostra como usar os métodos de auxiliar do Twitter de uma página em seu projeto. Em seu projeto, você desejará substituir os valores de parâmetros com valores que são relevantes para suas necessidades. Você pode usar as ids de widget fornecido para explorar como os métodos funcionam, mas você desejará gerar seus próprios widgets para seu projeto.

Nem todos os parâmetros mostrados abaixo são necessários. Os parâmetros opcionais são usados para personalizar como o botão ou widget é exibida. Por exemplo, o botão execute requer apenas o nome de usuário a seguir, mas o exemplo mostra como incluir o número de seguidores e como especificar o tamanho do botão e o idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Ver os resultados

O código acima produz os seguintes botões e widgets. Esses botões e widgets são totalmente funcional, não capturas de tela. Botão a seguir é exibido em espanhol, porque o parâmetro de idioma foi definido como **es**.

### <a name="follow-button"></a>Siga o botão

[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Botão de Tweet

[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Linha do tempo do usuário (perfil)

[TWEETS por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoritos

[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Lista

[TWEETS de @Microsoft/MS \_consumidor\_bandas](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Pesquisar

[Um Tweet sobre &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
