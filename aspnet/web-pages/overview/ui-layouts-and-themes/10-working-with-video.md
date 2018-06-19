---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: A exibição de vídeo em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs
author: tfitzmac
description: Este capítulo explica como exibir o vídeo em uma página da Web do ASP.NET com a página de sintaxe do Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153674"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>A exibição de vídeo em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um player de vídeo (mídia) em um site de páginas da Web do ASP.NET (Razor) para permitir que os usuários exibir vídeos que são armazenados no site. ASP.NET Web Pages com sintaxe Razor permite que você reproduza Flash (*. SWF*), Media Player (*. wmv*) e o Silverlight (*. xap*) vídeos.
> 
> O que você aprenderá:
> 
> - Como escolher um player de vídeo.
> - Como adicionar um vídeo a uma página da web.
> - Como definir atributos de player de vídeo.
> 
> Esses são o ASP.NET Razor páginas recursos introduzidos no artigo:
> 
> - O `Video` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3.


## <a name="introduction"></a>Introdução

Você talvez queira exibir um vídeo em seu site. Uma maneira de fazer isso é para vincular a um site que já tenha o vídeo, como o YouTube. Se você quiser inserir um vídeo desses sites diretamente no seus próprio páginas, pode obter geralmente marcação HTML do site e, em seguida, copie-o em sua página. Por exemplo, o exemplo a seguir mostra como inserir um YouTube vídeo:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se você quiser executar um vídeo de seu próprio site (não em um site público do compartilhamento de vídeo), você não pode vincular diretamente usando marcação incorporada como esse. No entanto, você pode reproduzir vídeos do seu site usando o `Video` auxiliar, que renderiza um reprodutor de mídia diretamente em uma página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Escolhendo um Player de vídeo

Há vários formatos de arquivos de vídeo, e cada formato normalmente requer um player diferente e outra maneira de configurar o player. Nas páginas do ASP.NET Razor, você pode executar um vídeo em uma página da web usando o `Video` auxiliar. O `Video` auxiliar simplifica o processo de incorporação de vídeos em uma página da web porque ela gera automaticamente o `object` e `embed` elementos HTML que normalmente são usados para adicionar um vídeo para a página.

O `Video` auxiliar suporta os seguintes players de mídia:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>O Player Flash

O `Flash` player do `Video` auxiliar lhe permite reproduzir vídeos Flash (*. SWF* arquivos) em uma página da web. No mínimo, você precisa fornecer um caminho para o arquivo de vídeo. Se você especificar apenas o caminho, o player usa valores padrão que são definidos pela versão atual do Flash. Configurações padrão comuns são:

- O vídeo é exibido usando o padrão de largura e altura e sem uma cor de plano de fundo.
- O vídeo é reproduzido automaticamente quando a página for carregada.
- O vídeo loops continuamente até que ele seja interrompido explicitamente.
- O vídeo será dimensionado para mostrar todo o vídeo, em vez de cortar o vídeo de acordo com um tamanho específico.
- O vídeo é reproduzido em uma janela.

### <a name="the-mediaplayer-player"></a>O Player do Media Player

O `MediaPlayer` player do `Video` auxiliar permite reproduzir vídeos do Windows Media (*. wmv* arquivos), Windows Media audio (*. wma* arquivos) e MP3 (*. mp3* arquivos) em uma página da web. Você deve incluir o caminho do arquivo de mídia para reproduzir; todos os outros parâmetros são opcionais. Se você especificar apenas um caminho, o player usa as configurações padrão definidas pela versão atual do Media Player, tais como:

- O vídeo é exibido usando o padrão de largura e altura.
- O vídeo é reproduzido automaticamente quando a página for carregada.
- O vídeo é reproduzido uma vez (não loop).
- O player exibe o conjunto completo de controles na interface do usuário.
- O vídeo é reproduzido em uma janela.

### <a name="the-silverlight-player"></a>O Player do Silverlight

O `Silverlight` player do `Video` auxiliar permite que você executar o Windows Media Video (*. wmv* arquivos), Windows Media Audio (*. wma* arquivos) e MP3 (*. mp3* arquivos). Você deve definir o parâmetro path para apontar para um pacote de aplicativo baseado em Silverlight (*. xap* arquivo). Você também deve definir os parâmetros de largura e altura. Todos os outros parâmetros são opcionais. Quando você usar o player do Silverlight para vídeo, se você definir somente os parâmetros necessários, o player do Silverlight exibe o vídeo sem uma cor de plano de fundo.

> [!NOTE]
> Caso você não souber Silverlight: o *. xap* é um arquivo compactado que contém instruções de layout em um *. XAML* arquivo, o código gerenciado em assemblies e recursos opcionais. Você pode criar um *. xap* arquivo no Visual Studio como um projeto de aplicativo do Silverlight.


O `Silverlight` player de vídeo usa tanto as configurações que você fornecer para o player e as configurações que são fornecidas no *. xap* arquivo.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos MIME
> 
> Quando um navegador baixa um arquivo, o navegador torna-se de que o tipo de arquivo corresponde ao tipo MIME especificado para o documento que está sendo processado. O tipo MIME é o tipo de mídia ou tipo de conteúdo de um arquivo. O `Video` auxiliar usa os seguintes tipos MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reprodução de vídeos Flash (. SWF)

Este procedimento mostra como executar um vídeo Flash denominado *sample.swf*. O procedimento presume que você tem uma pasta chamada *mídia* em seu site e que o *. SWF* arquivo está nessa pasta.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.
2. No site, adicione uma página e denomine- *FlashVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Execute a página em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.) A página é exibida e o vídeo é reproduzido automaticamente. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Você pode definir o `quality` parâmetro de um vídeo Flash `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Você pode alterar o Flash vídeo seja reproduzido em um tamanho específico usando o `scale` parâmetro, que pode ser definido como o seguinte:

- `showall`. Isso torna o vídeo inteiro visíveis, mantendo a taxa de proporção original. No entanto, você pode acabar com bordas em cada lado.
- `noorder`. Isso dimensiona o vídeo, mantendo a taxa de proporção original, mas ele pode ser cortado.
- `exactfit`. Isso torna o vídeo inteiro visível sem precisar preservar a taxa de proporção original, mas pode ocorrer distorção.

Se você não especificar um `scale` parâmetro, o vídeo inteiro será visível e a taxa de proporção original será mantida sem qualquer corte. O exemplo a seguir mostra como usar o `scale` parâmetro:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

O Flash player oferece suporte a um configuração nomeada de modo de vídeo `windowMode`. Você pode definir isso como `window`, `opaque`, e `transparent`. Por padrão, o `windowMode` é definido como `window`, que exibe o vídeo em uma janela separada na página da web. O `opaque` configuração oculta tudo atrás de vídeo na página da web. O `transparent` configuração permite que o plano de fundo da página da web mostrar o vídeo, supondo que qualquer parte do vídeo é transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reproduzindo Media Player (*. wmv*) vídeos

O procedimento a seguir mostra como reproduzir um vídeo de janela mídia denominado *sample.wmv* no *mídia* pasta.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.
2. Criar uma nova página chamada *MediaPlayerVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Execute a página em um navegador. O vídeo carrega e executa automaticamente. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Você pode definir `playCount` para um inteiro que indica quantas vezes para reproduzir o vídeo automaticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

O `uiMode` parâmetro permite que você especifique quais controles aparecem na interface do usuário. Você pode definir `uiMode` para `invisible`, `none`, `mini`, ou `full`. Se você não especificar um `uiMode` parâmetro, o vídeo será exibido com a janela de status, busca barras, controlar botões e controles de volume, além de janela de vídeo. Esses controles também serão exibidos se você usar o player para reproduzir um arquivo de áudio. Aqui está um exemplo de como usar o `uiMode` parâmetro:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Por padrão, áudio está ativado, quando o vídeo é reproduzido. Você pode desabilitar o áudio, definindo o `mute` como verdadeiro:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Você pode controlar o nível de áudio do vídeo Media Player, definindo o `volume` parâmetro para um valor entre 0 e 100. O valor padrão é 50. Veja um exemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reprodução de vídeos do Silverlight

Este procedimento mostra como reproduzir o vídeo contido em um Silverlight *. xap* página em uma pasta denominada *mídia*.

1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.
2. Criar uma nova página chamada *SilverlightVideo.cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Execute a página em um navegador. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Visão geral do Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de marca de objeto e a inserção de Flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Marcas PARAM de SDK do Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
