---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Exibindo vídeo em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este capítulo explica como exibir o vídeo em uma páginas da Web do ASP.NET com a página de sintaxe do Razor.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 28884d8c4998ea5b00a60bf094f41b915b565bd8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824812"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>A exibição de vídeo em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um player de vídeo (mídia) em um site de páginas da Web do ASP.NET (Razor) para permitir aos usuários exibir vídeos que são armazenados no site. ASP.NET Web Pages com sintaxe do Razor permite que você reproduza Flash (*. SWF*), o Player de mídia (*WMV*) e o Silverlight (*. xap*) vídeos.
> 
> O que você aprenderá:
> 
> - Como escolher um player de vídeo.
> - Como adicionar vídeo a uma página da web.
> - Como definir atributos de player de vídeo.
> 
> Esses são o ASP.NET Razor páginas recursos apresentados neste artigo:
> 
> - O `Video` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3.


## <a name="introduction"></a>Introdução

Você talvez queira exibir um vídeo no seu site. Uma maneira de fazer isso é para vincular a um site que já tenha o vídeo, como o YouTube. Se você deseja incorporar um vídeo desses sites diretamente em suas próprias páginas, poderá geralmente obtém a marcação HTML do site e, em seguida, copiá-lo para sua página. Por exemplo, o exemplo a seguir mostra como inserir um YouTube vídeo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se você quiser reproduzir um vídeo que está em seu próprio site (não em um site público do compartilhamento de vídeo), é possível vincular diretamente a ele usando marcação incorporada como essa. No entanto, você pode reproduzir vídeos do seu site usando o `Video` auxiliar, que renderiza um player de mídia diretamente em uma página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Escolhendo um Player de vídeo

Há muitos formatos de arquivos de vídeo, e cada formato normalmente requer um outro player e uma maneira diferente para configurar o player. Nas páginas do Razor do ASP.NET, você pode reproduzir um vídeo em uma página da web usando o `Video` auxiliar. O `Video` auxiliar simplifica o processo de incorporação de vídeos em uma página da web porque ele gera automaticamente o `object` e `embed` elementos HTML que normalmente são usados para adicionar vídeo à página.

O `Video` auxiliar suporta os seguintes players de mídia:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>O Flash Player

O `Flash` player do `Video` auxiliar permitem que você reproduza vídeos Flash (*. SWF* arquivos) em uma página da web. No mínimo, você precisa fornecer um caminho para o arquivo de vídeo. Se você especificar nada, mas o caminho, o player usa valores padrão que são definidos com a versão atual do Flash. Configurações padrão típicos são:

- O vídeo é exibido usando sua altura e largura padrão e sem uma cor de fundo.
- O vídeo é reproduzido automaticamente quando a página for carregada.
- O vídeo loops continuamente até que seja explicitamente interrompido.
- O vídeo será dimensionado para mostrar todo o vídeo, em vez de cortar o vídeo de acordo com um tamanho específico.
- O vídeo é reproduzido em uma janela.

### <a name="the-mediaplayer-player"></a>O Player de Media Player

O `MediaPlayer` player do `Video` auxiliar permite que você reproduza vídeos de mídia do Windows (*. wmv* arquivos), Windows Media audio (*WMA* arquivos) e MP3 (*. mp3* arquivos) em uma página da web. Você deve incluir o caminho do arquivo de mídia para reproduzir; todos os outros parâmetros são opcionais. Se você especificar apenas um caminho, o player usa configurações padrão definidas pela versão atual do MediaPlayer, tais como:

- O vídeo é exibido usando sua altura e largura padrão.
- O vídeo é reproduzido automaticamente quando a página for carregada.
- O vídeo é reproduzido uma vez (não loop).
- O player exibe o conjunto completo de controles na interface do usuário.
- O vídeo é reproduzido em uma janela.

### <a name="the-silverlight-player"></a>O Player do Silverlight

O `Silverlight` player do `Video` auxiliar permite que você reproduza o vídeo de mídia do Windows (*. wmv* arquivos), Windows Media Audio (*WMA* arquivos) e MP3 (*. mp3* arquivos). Você deve definir o parâmetro path para apontar para um pacote de aplicativo baseado em Silverlight (*. xap* arquivo). Você também deve definir os parâmetros de largura e altura. Todos os outros parâmetros são opcionais. Quando você usar o player do Silverlight para vídeo, se você definir somente os parâmetros necessários, o player do Silverlight exibe o vídeo sem uma cor de fundo.

> [!NOTE]
> Caso você ainda não souber o Silverlight: o *. xap* arquivo é um arquivo compactado que contém instruções de layout em um *. XAML* de arquivo, código gerenciado em assemblies e recursos opcionais. Você pode criar uma *. xap* arquivo no Visual Studio como um projeto de aplicativo do Silverlight.


O `Silverlight` player de vídeo usa tanto as configurações que você fornecer para o jogador e as configurações que são fornecidas na *. xap* arquivo.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos de MIME
> 
> Quando um navegador baixa um arquivo, o navegador faz-se de que o tipo de arquivo corresponde ao tipo MIME especificado para o documento que está sendo renderizado. O tipo MIME é o tipo de mídia ou o tipo de conteúdo de um arquivo. O `Video` auxiliar usa os seguintes tipos MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reprodução de vídeo Flash (. SWF)

Este procedimento mostra como reproduzir um vídeo em Flash denominado *sample.swf*. O procedimento presume que você tem uma pasta chamada *Media* em seu site e que o *. SWF* arquivo está nessa pasta.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.
2. No site, adicione uma página e nomeie- *FlashVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Execute a página em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) A página é exibida e o vídeo é reproduzido automaticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Você pode definir as `quality` parâmetro de um vídeo Flash `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Você pode alterar o vídeo em Flash para reproduzir em um tamanho específico usando o `scale` parâmetro, que pode ser definido para o seguinte:

- `showall`. Isso torna o vídeo inteiro visíveis, mantendo a taxa de proporção original. No entanto, você pode acabar com bordas em cada lado.
- `noorder`. Isso pode ser dimensionado com o vídeo, mantendo a taxa de proporção original, mas ele pode ser recortado.
- `exactfit`. Isso torna o vídeo inteiro visível sem precisar preservar a taxa de proporção original, mas pode ocorrer distorção.

Se você não especificar um `scale` parâmetro, o vídeo inteiro ficará visível e a taxa de proporção original será mantida sem qualquer corte. O exemplo a seguir mostra como usar o `scale` parâmetro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

O Flash player dá suporte a um modo de vídeo configuração chamada `windowMode`. Você pode definir isso como `window`, `opaque`, e `transparent`. Por padrão, o `windowMode` é definido como `window`, que exibe vídeo em uma janela separada, na página da web. O `opaque` configuração oculta tudo por trás de vídeo na página da web. O `transparent` configuração permite que o plano de fundo da página da web apareçam através de vídeo, supondo que qualquer parte do vídeo é transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reprodução de Media Player (*WMV*) vídeos

O procedimento a seguir mostra como reproduzir um vídeo de mídia de janela denominado *sample.wmv* que está no *mídia* pasta.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
2. Criar uma nova página chamada *MediaPlayerVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Execute a página em um navegador. O vídeo é carregado e é reproduzido automaticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Você pode definir `playCount` para um inteiro que indica quantas vezes para reproduzir o vídeo automaticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

O `uiMode` parâmetro permite que você especifique quais controles aparecem na interface do usuário. Você pode definir `uiMode` à `invisible`, `none`, `mini`, ou `full`. Se você não especificar um `uiMode` parâmetro, o vídeo será exibido com a janela de status, de busca de barras, controlar os botões e controles de volume além da janela de vídeo. Esses controles também serão exibidos se você usar o player para reproduzir um arquivo de áudio. Aqui está um exemplo de como usar o `uiMode` parâmetro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Por padrão, o áudio é no quando o vídeo é reproduzido. Você pode desabilitar o áudio, definindo o `mute` parâmetro como true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Você pode controlar o nível de áudio do vídeo MediaPlayer, definindo o `volume` parâmetro para um valor entre 0 e 100. O valor padrão é 50. Veja um exemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reprodução de vídeos do Silverlight

Este procedimento mostra como reproduzir o vídeo contido em um Silverlight *. xap* página que está em uma pasta chamada *mídia*.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não fez isso.
2. Criar uma nova página chamada *SilverlightVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Execute a página em um navegador. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Visão geral do Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de marca de objeto e a inserção de Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[As marcas PARAM de SDK do Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
