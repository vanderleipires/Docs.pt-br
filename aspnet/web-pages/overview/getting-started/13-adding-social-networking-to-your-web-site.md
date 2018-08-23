---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: A adição de redes sociais da Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Este capítulo explica como integrar o seu site com serviços de rede social. Neste capítulo, você aprenderá a permitir que as pessoas/link de indicador seu site...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 684fcfdde0aefeb168398bdf7a42f9fdbd6e48b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831621"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Adicionando rede Social para páginas da Web ASP.NET (Razor) Sites
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como adicionar links de rede social para Facebook, Twitter, Reddit e Digg para páginas em um site de páginas da Web do ASP.NET (Razor) e como incluir imagens Gravatar, cartões de jogador do Xbox e feeds do Twitter.
> 
> O que você aprenderá:
> 
> - Como permitir que as pessoas/link de indicador seu site.
> - Como adicionar um feed do Twitter.
> - Como adicionar um Facebook **como** botão às páginas.
> - Como renderizar imagens Gravatar.com.
> - Como exibir um cartão de jogador do Xbox em seu site.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - Biblioteca auxiliar da Web ASP.NET (pacote do NuGet)
>   
> 
> Este tutorial também funciona com 3 de páginas da Web do ASP.NET, exceto para as partes que usam a biblioteca de auxiliar de Web do ASP.NET.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Vinculando seu site em Sites de rede Social

Se as pessoas gostam de algo em seu site, muitas vezes querem compartilhá-lo com amigos. Você pode fazer isso fácil por meio da exibição de glifos (ícones) que pode ser clicado para compartilhar uma página no Digg, Reddit, Facebook, Twitter ou sites semelhantes.

Para exibir esses glifos, adicione o `LinkSharecode` auxiliar a uma página. As pessoas que visitam a página podem clicar em um glifo individuais. Se eles tiverem uma conta com esse site de rede social, eles podem, em seguida, postar um link para sua página no site em questão.

![Figura 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado proprietário. - criar uma página chamada *ListLinkShare.cshtml* e adicionar a marcação a seguir:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Neste exemplo, quando o `LinkShare` auxiliar execuções, o título da página é passado como um parâmetro, que por sua vez, passa o título da página para o site de rede social. No entanto, você pode passar qualquer cadeia de caracteres que você deseja. Este exemplo também especifica quais sites de redes sociais para incluir na lista. Você pode especificar os sites de rede social que são relevantes para seu site.
2. Execute o *ListLinkShare.cshtml* página em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.)
3. Clique em um glifo para um dos sites que você inscreveu-se em. O link leva você à página no site de rede social selecionado em que você pode compartilhar um link. Por exemplo, se você clicar no link do Reddit, você é levado para a `submit to reddit` página no site da Reddit.

     ![Figura 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Adicionar um Twitter Feed

Para obter informações sobre como usar um auxiliar do Twitter que é compatível com a versão atual da API do Twitter, consulte [auxiliar do Twitter](../ui-layouts-and-themes/twitter-helper.md). Este exemplo mostra como escrever seu próprio auxiliar para que possa reutilizar facilmente o código de várias páginas.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Exibindo um Facebook &quot;como&quot; botão

Em alguns casos, a melhor opção é obter o código diretamente do provedor de rede social em vez de depender de um auxiliar. Isso é especialmente verdadeiro se o provedor de rede social atualiza suas opções mais rapidamente do que o auxiliar é atualizado.

Para adicionar recursos do Facebook (por exemplo, o botão Like) em seu site, você pode recuperar os trechos de código do [developers.facebook.com](https://developers.facebook.com/) site. No site do Facebook, você pode usar suas ferramentas para gerar um trecho de código que é relevante para seu site.

O seguinte código realçado é o código que foi recuperado da ferramenta, como o botão no site de developers.facebook.com. Você deve fornecer sua própria ID do aplicativo.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Renderizar uma imagem de Gravatar

Um *Gravatar* (um &quot;avatar globalmente reconhecido&quot;) é uma imagem que pode ser usada em vários sites como seu avatar &#8212; , ou seja, uma imagem que representa a você. Por exemplo, um Gravatar pode identificar uma pessoa em uma postagem no fórum, em um comentário de blog e assim por diante. (Você pode registrar seu próprios Gravatar no site no Gravatar [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Se você quiser exibir imagens ao lado das pessoas nomes ou endereços de email em seu site, você pode usar o auxiliar Gravatar.

Neste exemplo, você está usando um único Gravatar que representa a mesmo. Outra maneira de usar um Gravatar é permitir que as pessoas especifique seu endereço de Gravatar quando eles se registram no seu site. (Você pode aprender como permitir que as pessoas se registrar no [adicionando segurança e associação a um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).) Em seguida, sempre que você exibe informações de que o usuário, você pode adicionar apenas o Gravatar para onde você pode exibir o nome do usuário.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
2. Criar uma nova página da web denominado *Gravatar.cshtml*.
3. Adicione a seguinte marcação ao arquivo: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    O `Gravatar.GetHtml` método exibe a imagem do Gravatar na página. Para alterar o tamanho da imagem, você pode incluir um número como um segundo parâmetro. O tamanho padrão é 80. Números de marca de menos de 80 a imagem menor. Números maiores que 80 tornar a imagem maior.
4. No `Gravatar.GetHtml` substitua métodos, `<Your Gravatar account here>` com o endereço de email que você usa para sua conta do Gravatar. (Se você não tiver uma conta de Gravatar, você pode usar o endereço de email de alguém que tenha.)
5. Execute a página em seu navegador. A página exibe duas imagens Gravatar para o endereço de email especificado. A segunda imagem é menor do que o primeiro. 

    ![Figura 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Exibindo um cartão de jogador do Xbox

Quando as pessoas jogam Xbox Microsoft online, cada usuário tem uma ID exclusiva. As estatísticas são mantidas para cada jogador na forma de um cartão de jogador, que mostra a sua reputação, a pontuação do jogador e executados recentemente jogos. Se você for um jogador do Xbox, você pode mostrar seu cartão de jogador nas páginas do seu site usando o `GamerCard` auxiliar.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
2. Criar uma nova página chamada *XboxGamer.cshtml* e adicione a marcação a seguir.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Você usa o `GamerCard.GetHtml` propriedade para especificar o alias para o jogador cartão a ser exibido.
3. Execute a página em seu navegador. A página exibe o cartão de jogador do Xbox que você especificou.

    ![Figura 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
