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
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d951a-103">A exibição de vídeo em um Site de páginas (Razor) da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d951a-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d951a-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d951a-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d951a-105">Este artigo explica como usar um player de vídeo (mídia) em um site de páginas da Web do ASP.NET (Razor) para permitir que os usuários exibir vídeos que são armazenados no site.</span><span class="sxs-lookup"><span data-stu-id="d951a-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="d951a-106">ASP.NET Web Pages com sintaxe Razor permite que você reproduza Flash (*. SWF*), Media Player (*. wmv*) e o Silverlight (*. xap*) vídeos.</span><span class="sxs-lookup"><span data-stu-id="d951a-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="d951a-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="d951a-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d951a-108">Como escolher um player de vídeo.</span><span class="sxs-lookup"><span data-stu-id="d951a-108">How to choose a video player.</span></span>
> - <span data-ttu-id="d951a-109">Como adicionar um vídeo a uma página da web.</span><span class="sxs-lookup"><span data-stu-id="d951a-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="d951a-110">Como definir atributos de player de vídeo.</span><span class="sxs-lookup"><span data-stu-id="d951a-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="d951a-111">Esses são o ASP.NET Razor páginas recursos introduzidos no artigo:</span><span class="sxs-lookup"><span data-stu-id="d951a-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="d951a-112">O `Video` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="d951a-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d951a-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="d951a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d951a-114">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d951a-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d951a-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="d951a-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="d951a-116">Este tutorial também funciona com o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="d951a-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="d951a-117">Introdução</span><span class="sxs-lookup"><span data-stu-id="d951a-117">Introduction</span></span>

<span data-ttu-id="d951a-118">Você talvez queira exibir um vídeo em seu site.</span><span class="sxs-lookup"><span data-stu-id="d951a-118">You might want to display a video on your site.</span></span> <span data-ttu-id="d951a-119">Uma maneira de fazer isso é para vincular a um site que já tenha o vídeo, como o YouTube.</span><span class="sxs-lookup"><span data-stu-id="d951a-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="d951a-120">Se você quiser inserir um vídeo desses sites diretamente no seus próprio páginas, pode obter geralmente marcação HTML do site e, em seguida, copie-o em sua página.</span><span class="sxs-lookup"><span data-stu-id="d951a-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="d951a-121">Por exemplo, o exemplo a seguir mostra como inserir um YouTube vídeo:</span><span class="sxs-lookup"><span data-stu-id="d951a-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="d951a-122">Se você quiser executar um vídeo de seu próprio site (não em um site público do compartilhamento de vídeo), você não pode vincular diretamente usando marcação incorporada como esse.</span><span class="sxs-lookup"><span data-stu-id="d951a-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="d951a-123">No entanto, você pode reproduzir vídeos do seu site usando o `Video` auxiliar, que renderiza um reprodutor de mídia diretamente em uma página.</span><span class="sxs-lookup"><span data-stu-id="d951a-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="d951a-124">Escolhendo um Player de vídeo</span><span class="sxs-lookup"><span data-stu-id="d951a-124">Choosing a Video Player</span></span>

<span data-ttu-id="d951a-125">Há vários formatos de arquivos de vídeo, e cada formato normalmente requer um player diferente e outra maneira de configurar o player.</span><span class="sxs-lookup"><span data-stu-id="d951a-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="d951a-126">Nas páginas do ASP.NET Razor, você pode executar um vídeo em uma página da web usando o `Video` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="d951a-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="d951a-127">O `Video` auxiliar simplifica o processo de incorporação de vídeos em uma página da web porque ela gera automaticamente o `object` e `embed` elementos HTML que normalmente são usados para adicionar um vídeo para a página.</span><span class="sxs-lookup"><span data-stu-id="d951a-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="d951a-128">O `Video` auxiliar suporta os seguintes players de mídia:</span><span class="sxs-lookup"><span data-stu-id="d951a-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="d951a-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="d951a-129">Adobe Flash</span></span>
- <span data-ttu-id="d951a-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="d951a-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="d951a-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="d951a-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="d951a-132">O Player Flash</span><span class="sxs-lookup"><span data-stu-id="d951a-132">The Flash Player</span></span>

<span data-ttu-id="d951a-133">O `Flash` player do `Video` auxiliar lhe permite reproduzir vídeos Flash (*. SWF* arquivos) em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="d951a-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="d951a-134">No mínimo, você precisa fornecer um caminho para o arquivo de vídeo.</span><span class="sxs-lookup"><span data-stu-id="d951a-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="d951a-135">Se você especificar apenas o caminho, o player usa valores padrão que são definidos pela versão atual do Flash.</span><span class="sxs-lookup"><span data-stu-id="d951a-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="d951a-136">Configurações padrão comuns são:</span><span class="sxs-lookup"><span data-stu-id="d951a-136">Typical default settings are:</span></span>

- <span data-ttu-id="d951a-137">O vídeo é exibido usando o padrão de largura e altura e sem uma cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="d951a-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="d951a-138">O vídeo é reproduzido automaticamente quando a página for carregada.</span><span class="sxs-lookup"><span data-stu-id="d951a-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="d951a-139">O vídeo loops continuamente até que ele seja interrompido explicitamente.</span><span class="sxs-lookup"><span data-stu-id="d951a-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="d951a-140">O vídeo será dimensionado para mostrar todo o vídeo, em vez de cortar o vídeo de acordo com um tamanho específico.</span><span class="sxs-lookup"><span data-stu-id="d951a-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="d951a-141">O vídeo é reproduzido em uma janela.</span><span class="sxs-lookup"><span data-stu-id="d951a-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="d951a-142">O Player do Media Player</span><span class="sxs-lookup"><span data-stu-id="d951a-142">The MediaPlayer Player</span></span>

<span data-ttu-id="d951a-143">O `MediaPlayer` player do `Video` auxiliar permite reproduzir vídeos do Windows Media (*. wmv* arquivos), Windows Media audio (*. wma* arquivos) e MP3 (*. mp3* arquivos) em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="d951a-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="d951a-144">Você deve incluir o caminho do arquivo de mídia para reproduzir; todos os outros parâmetros são opcionais.</span><span class="sxs-lookup"><span data-stu-id="d951a-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="d951a-145">Se você especificar apenas um caminho, o player usa as configurações padrão definidas pela versão atual do Media Player, tais como:</span><span class="sxs-lookup"><span data-stu-id="d951a-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="d951a-146">O vídeo é exibido usando o padrão de largura e altura.</span><span class="sxs-lookup"><span data-stu-id="d951a-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="d951a-147">O vídeo é reproduzido automaticamente quando a página for carregada.</span><span class="sxs-lookup"><span data-stu-id="d951a-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="d951a-148">O vídeo é reproduzido uma vez (não loop).</span><span class="sxs-lookup"><span data-stu-id="d951a-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="d951a-149">O player exibe o conjunto completo de controles na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d951a-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="d951a-150">O vídeo é reproduzido em uma janela.</span><span class="sxs-lookup"><span data-stu-id="d951a-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="d951a-151">O Player do Silverlight</span><span class="sxs-lookup"><span data-stu-id="d951a-151">The Silverlight Player</span></span>

<span data-ttu-id="d951a-152">O `Silverlight` player do `Video` auxiliar permite que você executar o Windows Media Video (*. wmv* arquivos), Windows Media Audio (*. wma* arquivos) e MP3 (*. mp3* arquivos).</span><span class="sxs-lookup"><span data-stu-id="d951a-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="d951a-153">Você deve definir o parâmetro path para apontar para um pacote de aplicativo baseado em Silverlight (*. xap* arquivo).</span><span class="sxs-lookup"><span data-stu-id="d951a-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="d951a-154">Você também deve definir os parâmetros de largura e altura.</span><span class="sxs-lookup"><span data-stu-id="d951a-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="d951a-155">Todos os outros parâmetros são opcionais.</span><span class="sxs-lookup"><span data-stu-id="d951a-155">All other parameters are optional.</span></span> <span data-ttu-id="d951a-156">Quando você usar o player do Silverlight para vídeo, se você definir somente os parâmetros necessários, o player do Silverlight exibe o vídeo sem uma cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="d951a-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="d951a-157">Caso você não souber Silverlight: o *. xap* é um arquivo compactado que contém instruções de layout em um *. XAML* arquivo, o código gerenciado em assemblies e recursos opcionais.</span><span class="sxs-lookup"><span data-stu-id="d951a-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="d951a-158">Você pode criar um *. xap* arquivo no Visual Studio como um projeto de aplicativo do Silverlight.</span><span class="sxs-lookup"><span data-stu-id="d951a-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="d951a-159">O `Silverlight` player de vídeo usa tanto as configurações que você fornecer para o player e as configurações que são fornecidas no *. xap* arquivo.</span><span class="sxs-lookup"><span data-stu-id="d951a-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="d951a-160">Tipos MIME</span><span class="sxs-lookup"><span data-stu-id="d951a-160">MIME Types</span></span>
> 
> <span data-ttu-id="d951a-161">Quando um navegador baixa um arquivo, o navegador torna-se de que o tipo de arquivo corresponde ao tipo MIME especificado para o documento que está sendo processado.</span><span class="sxs-lookup"><span data-stu-id="d951a-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="d951a-162">O tipo MIME é o tipo de mídia ou tipo de conteúdo de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="d951a-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="d951a-163">O `Video` auxiliar usa os seguintes tipos MIME:</span><span class="sxs-lookup"><span data-stu-id="d951a-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="d951a-164">Reprodução de vídeos Flash (. SWF)</span><span class="sxs-lookup"><span data-stu-id="d951a-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="d951a-165">Este procedimento mostra como executar um vídeo Flash denominado *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="d951a-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="d951a-166">O procedimento presume que você tem uma pasta chamada *mídia* em seu site e que o *. SWF* arquivo está nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="d951a-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="d951a-167">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.</span><span class="sxs-lookup"><span data-stu-id="d951a-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="d951a-168">No site, adicione uma página e denomine- *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d951a-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="d951a-169">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="d951a-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="d951a-170">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="d951a-170">Run the page in a browser.</span></span> <span data-ttu-id="d951a-171">(Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.) A página é exibida e o vídeo é reproduzido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d951a-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="d951a-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="d951a-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="d951a-173">Você pode definir o `quality` parâmetro de um vídeo Flash `low`, `autolow`, `autohigh`, `medium`, `high`, e `best`:</span><span class="sxs-lookup"><span data-stu-id="d951a-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="d951a-174">Você pode alterar o Flash vídeo seja reproduzido em um tamanho específico usando o `scale` parâmetro, que pode ser definido como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d951a-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="d951a-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="d951a-175">`showall`.</span></span> <span data-ttu-id="d951a-176">Isso torna o vídeo inteiro visíveis, mantendo a taxa de proporção original.</span><span class="sxs-lookup"><span data-stu-id="d951a-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="d951a-177">No entanto, você pode acabar com bordas em cada lado.</span><span class="sxs-lookup"><span data-stu-id="d951a-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="d951a-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="d951a-178">`noorder`.</span></span> <span data-ttu-id="d951a-179">Isso dimensiona o vídeo, mantendo a taxa de proporção original, mas ele pode ser cortado.</span><span class="sxs-lookup"><span data-stu-id="d951a-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="d951a-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="d951a-180">`exactfit`.</span></span> <span data-ttu-id="d951a-181">Isso torna o vídeo inteiro visível sem precisar preservar a taxa de proporção original, mas pode ocorrer distorção.</span><span class="sxs-lookup"><span data-stu-id="d951a-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="d951a-182">Se você não especificar um `scale` parâmetro, o vídeo inteiro será visível e a taxa de proporção original será mantida sem qualquer corte.</span><span class="sxs-lookup"><span data-stu-id="d951a-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="d951a-183">O exemplo a seguir mostra como usar o `scale` parâmetro:</span><span class="sxs-lookup"><span data-stu-id="d951a-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="d951a-184">O Flash player oferece suporte a um configuração nomeada de modo de vídeo `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="d951a-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="d951a-185">Você pode definir isso como `window`, `opaque`, e `transparent`.</span><span class="sxs-lookup"><span data-stu-id="d951a-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="d951a-186">Por padrão, o `windowMode` é definido como `window`, que exibe o vídeo em uma janela separada na página da web.</span><span class="sxs-lookup"><span data-stu-id="d951a-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="d951a-187">O `opaque` configuração oculta tudo atrás de vídeo na página da web.</span><span class="sxs-lookup"><span data-stu-id="d951a-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="d951a-188">O `transparent` configuração permite que o plano de fundo da página da web mostrar o vídeo, supondo que qualquer parte do vídeo é transparente.</span><span class="sxs-lookup"><span data-stu-id="d951a-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="d951a-189">Reproduzindo Media Player (*. wmv*) vídeos</span><span class="sxs-lookup"><span data-stu-id="d951a-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="d951a-190">O procedimento a seguir mostra como reproduzir um vídeo de janela mídia denominado *sample.wmv* no *mídia* pasta.</span><span class="sxs-lookup"><span data-stu-id="d951a-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="d951a-191">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.</span><span class="sxs-lookup"><span data-stu-id="d951a-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="d951a-192">Criar uma nova página chamada *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d951a-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="d951a-193">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="d951a-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="d951a-194">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="d951a-194">Run the page in a browser.</span></span> <span data-ttu-id="d951a-195">O vídeo carrega e executa automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d951a-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="d951a-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="d951a-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="d951a-197">Você pode definir `playCount` para um inteiro que indica quantas vezes para reproduzir o vídeo automaticamente:</span><span class="sxs-lookup"><span data-stu-id="d951a-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="d951a-198">O `uiMode` parâmetro permite que você especifique quais controles aparecem na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d951a-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="d951a-199">Você pode definir `uiMode` para `invisible`, `none`, `mini`, ou `full`.</span><span class="sxs-lookup"><span data-stu-id="d951a-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="d951a-200">Se você não especificar um `uiMode` parâmetro, o vídeo será exibido com a janela de status, busca barras, controlar botões e controles de volume, além de janela de vídeo.</span><span class="sxs-lookup"><span data-stu-id="d951a-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="d951a-201">Esses controles também serão exibidos se você usar o player para reproduzir um arquivo de áudio.</span><span class="sxs-lookup"><span data-stu-id="d951a-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="d951a-202">Aqui está um exemplo de como usar o `uiMode` parâmetro:</span><span class="sxs-lookup"><span data-stu-id="d951a-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="d951a-203">Por padrão, áudio está ativado, quando o vídeo é reproduzido.</span><span class="sxs-lookup"><span data-stu-id="d951a-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="d951a-204">Você pode desabilitar o áudio, definindo o `mute` como verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="d951a-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="d951a-205">Você pode controlar o nível de áudio do vídeo Media Player, definindo o `volume` parâmetro para um valor entre 0 e 100.</span><span class="sxs-lookup"><span data-stu-id="d951a-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="d951a-206">O valor padrão é 50.</span><span class="sxs-lookup"><span data-stu-id="d951a-206">The default value is 50.</span></span> <span data-ttu-id="d951a-207">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="d951a-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="d951a-208">Reprodução de vídeos do Silverlight</span><span class="sxs-lookup"><span data-stu-id="d951a-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="d951a-209">Este procedimento mostra como reproduzir o vídeo contido em um Silverlight *. xap* página em uma pasta denominada *mídia*.</span><span class="sxs-lookup"><span data-stu-id="d951a-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="d951a-210">Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não o fez.</span><span class="sxs-lookup"><span data-stu-id="d951a-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="d951a-211">Criar uma nova página chamada *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d951a-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="d951a-212">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="d951a-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="d951a-213">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="d951a-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="d951a-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="d951a-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d951a-215">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="d951a-215">Additional Resources</span></span>


<span data-ttu-id="d951a-216">[Visão geral do Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="d951a-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="d951a-217">Atributos de marca de objeto e a inserção de Flash</span><span class="sxs-lookup"><span data-stu-id="d951a-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="d951a-218">[Marcas PARAM de SDK do Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="d951a-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
