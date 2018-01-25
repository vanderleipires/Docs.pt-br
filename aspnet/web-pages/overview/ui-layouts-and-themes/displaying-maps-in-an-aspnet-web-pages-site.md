---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: "Exibir mapas em uma Web ASP.NET páginas Site (Razor) | Microsoft Docs"
author: tfitzmac
description: "Este artigo explica como exibir mapas interativos em páginas em um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b32c3-103">Exibição de mapas em um Site de páginas (Razor) da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b32c3-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b32c3-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b32c3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b32c3-105">Este artigo explica como exibir mapas interativos em páginas de um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="b32c3-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="b32c3-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="b32c3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b32c3-107">Como gerar um mapa com base em um endereço.</span><span class="sxs-lookup"><span data-stu-id="b32c3-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="b32c3-108">Como gerar um mapa com base em coordenadas de latitude e longitude.</span><span class="sxs-lookup"><span data-stu-id="b32c3-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="b32c3-109">Como registrar uma conta de desenvolvedor do Bing Maps e obter uma chave para usar com o Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="b32c3-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="b32c3-110">Este é o recurso ASP.NET introduzido no artigo:</span><span class="sxs-lookup"><span data-stu-id="b32c3-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="b32c3-111">O `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b32c3-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="b32c3-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b32c3-113">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="b32c3-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="b32c3-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="b32c3-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="b32c3-115">Este tutorial também funciona com o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="b32c3-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="b32c3-116">Em páginas da Web, você pode exibir mapas em uma página usando `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="b32c3-117">Você pode gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude.</span><span class="sxs-lookup"><span data-stu-id="b32c3-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="b32c3-118">O `Maps` classe permite que você chama mecanismos de mapa populares, incluindo Yahoo, Google, MapQuest e Bing.</span><span class="sxs-lookup"><span data-stu-id="b32c3-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="b32c3-119">As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="b32c3-120">Adicione uma referência de arquivo JavaScript que faz com que os métodos disponíveis para exibir o mapa e, em seguida, você pode chamar métodos do `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="b32c3-121">Escolha um serviço de mapa com base no qual `Maps` método auxiliar usado.</span><span class="sxs-lookup"><span data-stu-id="b32c3-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="b32c3-122">Você pode usar um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="b32c3-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="b32c3-123">Instalar as partes que necessárias</span><span class="sxs-lookup"><span data-stu-id="b32c3-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="b32c3-124">Para exibir mapas, você precisa estas partes:</span><span class="sxs-lookup"><span data-stu-id="b32c3-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="b32c3-125">O `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-125">The `Maps` helper.</span></span> <span data-ttu-id="b32c3-126">Este auxiliar é a versão 2 do ASP.NET Web Helpers Library.</span><span class="sxs-lookup"><span data-stu-id="b32c3-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="b32c3-127">Se você ainda não adicionou a biblioteca, você pode instalá-lo em seu site como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="b32c3-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="b32c3-128">Para obter detalhes, consulte [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="b32c3-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="b32c3-129">(Na galeria, procure o `microsoft-web-helpers` pacote.)</span><span class="sxs-lookup"><span data-stu-id="b32c3-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="b32c3-130">A biblioteca jQuery.</span><span class="sxs-lookup"><span data-stu-id="b32c3-130">The jQuery library.</span></span> <span data-ttu-id="b32c3-131">Vários dos modelos de site do WebMatrix já incluem bibliotecas jQuery em seus *Script* pastas.</span><span class="sxs-lookup"><span data-stu-id="b32c3-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="b32c3-132">Se você não tiver essas bibliotecas, você pode baixar a biblioteca mais recente do jQuery diretamente a partir de [jQuery.org](http://jQuery.org) site.</span><span class="sxs-lookup"><span data-stu-id="b32c3-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="b32c3-133">Ou você pode criar um novo site usando um modelo (por exemplo, o **Site inicial** modelo) e, em seguida, copie os arquivos de jQuery do site para o site atual.</span><span class="sxs-lookup"><span data-stu-id="b32c3-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="b32c3-134">Finalmente, se você quiser usar o Bing maps, você deve primeiro criar uma conta (gratuita) e obter uma chave.</span><span class="sxs-lookup"><span data-stu-id="b32c3-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="b32c3-135">Para obter uma chave, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="b32c3-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="b32c3-136">Criar uma conta de [conta de desenvolvedor do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="b32c3-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="b32c3-137">Você deve ter uma conta da Microsoft (Windows Live ID) também.</span><span class="sxs-lookup"><span data-stu-id="b32c3-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="b32c3-138">Você pode especificar que você deseja usar a chave para **avaliação e teste**.</span><span class="sxs-lookup"><span data-stu-id="b32c3-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="b32c3-139">Se você estiver testando a função de mapeamento no seu computador usando o WebMatrix e o IIS Express, visite o **Site** espaço de trabalho e anote a URL do seu site (por exemplo, `http://localhost:50408`, embora o número de porta provavelmente será diferente).</span><span class="sxs-lookup"><span data-stu-id="b32c3-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="b32c3-140">Você pode usar isso *localhost* endereço do site quando você registrar.</span><span class="sxs-lookup"><span data-stu-id="b32c3-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="b32c3-141">Depois que você registrou para uma conta, vá para o Centro de contas do Bing Maps e clique em **criar ou exibir chaves**:</span><span class="sxs-lookup"><span data-stu-id="b32c3-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="b32c3-143">Registre a chave que cria Bing.</span><span class="sxs-lookup"><span data-stu-id="b32c3-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="b32c3-144">Criar um mapa com base em um endereço (usando o Google)</span><span class="sxs-lookup"><span data-stu-id="b32c3-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="b32c3-145">O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço.</span><span class="sxs-lookup"><span data-stu-id="b32c3-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="b32c3-146">Este exemplo mostra como usar o Google Maps.</span><span class="sxs-lookup"><span data-stu-id="b32c3-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="b32c3-147">Crie um arquivo chamado *MapAddress.cshtml* na raiz do site.</span><span class="sxs-lookup"><span data-stu-id="b32c3-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="b32c3-148">Esta página irá gerar um mapa com base em um endereço que você passa para ele.</span><span class="sxs-lookup"><span data-stu-id="b32c3-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="b32c3-149">Copie o código a seguir para o arquivo, substituindo o conteúdo existente.</span><span class="sxs-lookup"><span data-stu-id="b32c3-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="b32c3-150">Observe os seguintes recursos da página:</span><span class="sxs-lookup"><span data-stu-id="b32c3-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="b32c3-151">O `<script>` elemento o `<head>` elemento.</span><span class="sxs-lookup"><span data-stu-id="b32c3-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="b32c3-152">No exemplo, o `<script>` referências de elementos de *1.6.4.min.js jquery* arquivo, que é uma versão minimizada (compactada) da biblioteca do jQuery, versão 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="b32c3-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="b32c3-153">Observe que a referência supõe que o *. js* arquivo está no *Scripts* pasta do site.</span><span class="sxs-lookup"><span data-stu-id="b32c3-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="b32c3-154">Se você estiver usando uma versão diferente da biblioteca jQuery, apenas certifique-se de que você está apontando para a versão corretamente.</span><span class="sxs-lookup"><span data-stu-id="b32c3-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="b32c3-155">A chamada para o `@Maps.GetGoogleHtml` no corpo da página.</span><span class="sxs-lookup"><span data-stu-id="b32c3-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="b32c3-156">Para mapear um endereço, você deve passar uma cadeia de caracteres do endereço.</span><span class="sxs-lookup"><span data-stu-id="b32c3-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="b32c3-157">Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="b32c3-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
- <span data-ttu-id="b32c3-158">Execute a página e digite um endereço.</span><span class="sxs-lookup"><span data-stu-id="b32c3-158">Run the page and enter an address.</span></span> <span data-ttu-id="b32c3-159">A página exibe um mapa, com base no Google Maps, que mostra o local especificado.</span><span class="sxs-lookup"><span data-stu-id="b32c3-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

    ![mapeamento de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="b32c3-161">Criar um mapa com base em Latitude e Longitude coordenadas (usando o Bing)</span><span class="sxs-lookup"><span data-stu-id="b32c3-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="b32c3-162">Este exemplo mostra como criar um mapa com base nas coordenadas.</span><span class="sxs-lookup"><span data-stu-id="b32c3-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="b32c3-163">Este exemplo mostra como usar o Bing maps e como incluir a chave do Bing.</span><span class="sxs-lookup"><span data-stu-id="b32c3-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="b32c3-164">(Você pode criar um mapa com base nas coordenadas usando mecanismos de mapa Além disso, sem usar uma chave do Bing).</span><span class="sxs-lookup"><span data-stu-id="b32c3-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="b32c3-165">Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site e substituir o conteúdo existente com o seguinte código e marcação:</span><span class="sxs-lookup"><span data-stu-id="b32c3-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="b32c3-166">Substituir `your-key-here` com a chave do Bing Maps que você gerou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b32c3-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="b32c3-167">Execute o *MapCoordinates.cshtml* página, insira as coordenadas de latitude e longitude e, em seguida, clique no **mapa ele!**</span><span class="sxs-lookup"><span data-stu-id="b32c3-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="b32c3-168">.</span><span class="sxs-lookup"><span data-stu-id="b32c3-168">button.</span></span> <span data-ttu-id="b32c3-169">(Se você não souber qualquer coordenadas, tente o seguinte.</span><span class="sxs-lookup"><span data-stu-id="b32c3-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="b32c3-170">Esse é um local no campus da Microsoft Redmond.)</span><span class="sxs-lookup"><span data-stu-id="b32c3-170">This is a location on the Microsoft Redmond campus.)</span></span>

    - <span data-ttu-id="b32c3-171">Latitude: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="b32c3-171">Latitude: 47.6781005859375</span></span>
    - <span data-ttu-id="b32c3-172">Longitude: -122.158317565918</span><span class="sxs-lookup"><span data-stu-id="b32c3-172">Longitude: -122.158317565918</span></span>

    <span data-ttu-id="b32c3-173">A página é exibida usando as coordenadas especificadas.</span><span class="sxs-lookup"><span data-stu-id="b32c3-173">The page is displayed using the coordinates that you specified.</span></span>

    ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b32c3-175">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="b32c3-175">Additional Resources</span></span>


[<span data-ttu-id="b32c3-176">Referência da API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="b32c3-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
