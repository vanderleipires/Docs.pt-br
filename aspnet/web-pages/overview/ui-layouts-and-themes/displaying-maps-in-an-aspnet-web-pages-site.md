---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Exibição de mapas em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo explica como exibir mapas interativos em páginas em um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368829"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="54473-103">Exibição de mapas em um Site do ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="54473-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="54473-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="54473-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="54473-105">Este artigo explica como exibir mapas interativos em páginas em um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="54473-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="54473-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="54473-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="54473-107">Como gerar um mapa com base em um endereço.</span><span class="sxs-lookup"><span data-stu-id="54473-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="54473-108">Como gerar um mapa com base nas coordenadas de latitude e longitude.</span><span class="sxs-lookup"><span data-stu-id="54473-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="54473-109">Como registrar uma conta de desenvolvedor do Bing Maps e obter uma chave para uso com o Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="54473-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="54473-110">Esse é o recurso ASP.NET introduzido no artigo:</span><span class="sxs-lookup"><span data-stu-id="54473-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="54473-111">O `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="54473-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="54473-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="54473-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="54473-113">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="54473-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="54473-114">O WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="54473-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="54473-115">Este tutorial também funciona com o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="54473-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="54473-116">Em páginas da Web, você pode exibir mapas em uma página usando `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="54473-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="54473-117">Você pode gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude.</span><span class="sxs-lookup"><span data-stu-id="54473-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="54473-118">O `Maps` classe permite que você chame em mecanismos de mapa populares, incluindo Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="54473-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="54473-119">As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar.</span><span class="sxs-lookup"><span data-stu-id="54473-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="54473-120">Basta adicionar uma referência de arquivo JavaScript que faz os métodos disponíveis para exibir o mapa e, em seguida, chamar métodos do `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="54473-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="54473-121">Um serviço de mapa com base no qual você escolha `Maps` método auxiliar que você usar.</span><span class="sxs-lookup"><span data-stu-id="54473-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="54473-122">Você pode usar qualquer um destes:</span><span class="sxs-lookup"><span data-stu-id="54473-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="54473-123">Instalando as partes que necessárias</span><span class="sxs-lookup"><span data-stu-id="54473-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="54473-124">Para exibir mapas, você precisa ter essas partes:</span><span class="sxs-lookup"><span data-stu-id="54473-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="54473-125">O `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="54473-125">The `Maps` helper.</span></span> <span data-ttu-id="54473-126">Esse auxiliar está na versão 2 do ASP.NET Web Helpers Library.</span><span class="sxs-lookup"><span data-stu-id="54473-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="54473-127">Se você ainda não adicionou a biblioteca, você pode instalá-lo em seu site como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="54473-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="54473-128">Para obter detalhes, consulte [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="54473-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="54473-129">(Na galeria, pesquise o `microsoft-web-helpers` pacote.)</span><span class="sxs-lookup"><span data-stu-id="54473-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="54473-130">A biblioteca jQuery.</span><span class="sxs-lookup"><span data-stu-id="54473-130">The jQuery library.</span></span> <span data-ttu-id="54473-131">Vários dos modelos de site do WebMatrix já incluem bibliotecas do jQuery em sua *Script* pastas.</span><span class="sxs-lookup"><span data-stu-id="54473-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="54473-132">Se você não tiver essas bibliotecas, você pode baixar a biblioteca mais recente de jQuery diretamente a partir de [jQuery.org](http://jQuery.org) site.</span><span class="sxs-lookup"><span data-stu-id="54473-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="54473-133">Ou você pode criar um novo site usando um modelo (por exemplo, o **Starter Site** modelo) e, em seguida, copie os arquivos de jQuery desse site para seu site atual.</span><span class="sxs-lookup"><span data-stu-id="54473-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="54473-134">Por fim, se você quiser usar o Bing maps, você deve primeiro criar uma conta (gratuita) e obter uma chave.</span><span class="sxs-lookup"><span data-stu-id="54473-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="54473-135">Para obter uma chave, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="54473-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="54473-136">Criar uma conta de [conta de desenvolvedor do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="54473-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="54473-137">Você deve ter uma conta da Microsoft (Windows Live ID) também.</span><span class="sxs-lookup"><span data-stu-id="54473-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="54473-138">Você pode especificar que você deseja usar a chave para **avaliação e teste**.</span><span class="sxs-lookup"><span data-stu-id="54473-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="54473-139">Se você estiver testando a função de mapeamento no seu próprio computador usando o WebMatrix e o IIS Express, acesse o **Site** espaço de trabalho e anote a URL do seu site (por exemplo, `http://localhost:50408`, embora o número da porta provavelmente será diferente).</span><span class="sxs-lookup"><span data-stu-id="54473-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="54473-140">Você pode usá-lo *localhost* endereço como o site quando você se registra.</span><span class="sxs-lookup"><span data-stu-id="54473-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="54473-141">Depois de registrar para uma conta, vá para o Bing Maps Account Center e clique em **criar ou exibir as chaves**:</span><span class="sxs-lookup"><span data-stu-id="54473-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapeamento de-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="54473-143">Registre a chave do Bing cria.</span><span class="sxs-lookup"><span data-stu-id="54473-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="54473-144">Criar um mapa com base em um endereço (usando o Google)</span><span class="sxs-lookup"><span data-stu-id="54473-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="54473-145">O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço.</span><span class="sxs-lookup"><span data-stu-id="54473-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="54473-146">Este exemplo mostra como usar o Google Maps.</span><span class="sxs-lookup"><span data-stu-id="54473-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="54473-147">Crie um arquivo chamado *MapAddress.cshtml* na raiz do site.</span><span class="sxs-lookup"><span data-stu-id="54473-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="54473-148">Nesta página irá gerar um mapa com base em um endereço que você passa para ele.</span><span class="sxs-lookup"><span data-stu-id="54473-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="54473-149">Copie o código a seguir para o arquivo, substituindo o conteúdo existente.</span><span class="sxs-lookup"><span data-stu-id="54473-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="54473-150">Observe os seguintes recursos da página:</span><span class="sxs-lookup"><span data-stu-id="54473-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="54473-151">O `<script>` elemento no `<head>` elemento.</span><span class="sxs-lookup"><span data-stu-id="54473-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="54473-152">No exemplo, o `<script>` referências de elemento de *jquery 1.6.4.min.js* arquivo, que é uma versão minificada (compactada) da biblioteca jQuery, versão 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="54473-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="54473-153">Observe que a referência supõe que o *. js* arquivo está sendo a *Scripts* pasta do site.</span><span class="sxs-lookup"><span data-stu-id="54473-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="54473-154">Se você estiver usando uma versão diferente da biblioteca jQuery, apenas certifique-se de que você está apontando para a mesma versão corretamente.</span><span class="sxs-lookup"><span data-stu-id="54473-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="54473-155">A chamada para o `@Maps.GetGoogleHtml` no corpo da página.</span><span class="sxs-lookup"><span data-stu-id="54473-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="54473-156">Para mapear um endereço, você deve passar uma cadeia de caracteres de endereço.</span><span class="sxs-lookup"><span data-stu-id="54473-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="54473-157">Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="54473-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="54473-158">Execute a página e digite um endereço.</span><span class="sxs-lookup"><span data-stu-id="54473-158">Run the page and enter an address.</span></span> <span data-ttu-id="54473-159">A página exibe um mapa, com base em Google Maps, que mostra o local que você especificou.</span><span class="sxs-lookup"><span data-stu-id="54473-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapeamento de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="54473-161">Criar um mapa com base em Latitude e Longitude coordena (usando o Bing)</span><span class="sxs-lookup"><span data-stu-id="54473-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="54473-162">Este exemplo mostra como criar um mapa com base nas coordenadas.</span><span class="sxs-lookup"><span data-stu-id="54473-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="54473-163">Este exemplo mostra como usar o Bing maps e como incluir sua chave do Bing.</span><span class="sxs-lookup"><span data-stu-id="54473-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="54473-164">(Você pode criar um mapa com base nas coordenadas usando outros mecanismos de mapa também, sem usar uma chave do Bing).</span><span class="sxs-lookup"><span data-stu-id="54473-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="54473-165">Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site e substitua o conteúdo existente com o código e marcação a seguir:</span><span class="sxs-lookup"><span data-stu-id="54473-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="54473-166">Substitua `your-key-here` com a chave do Bing mapas que você gerou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="54473-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="54473-167">Execute o *MapCoordinates.cshtml* página, insira as coordenadas de latitude e longitude e, em seguida, clique no **mapa-lo!**</span><span class="sxs-lookup"><span data-stu-id="54473-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="54473-168">.</span><span class="sxs-lookup"><span data-stu-id="54473-168">button.</span></span> <span data-ttu-id="54473-169">(Se você não souber nenhum coordenadas, tente o seguinte.</span><span class="sxs-lookup"><span data-stu-id="54473-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="54473-170">Esse é um local no campus da Microsoft Redmond.)</span><span class="sxs-lookup"><span data-stu-id="54473-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="54473-171">Latitude: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="54473-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="54473-172">Longitude:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="54473-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="54473-173">A página é exibida usando as coordenadas que você especificou.</span><span class="sxs-lookup"><span data-stu-id="54473-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapeamento de-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="54473-175">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="54473-175">Additional Resources</span></span>


[<span data-ttu-id="54473-176">Referência da API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="54473-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
