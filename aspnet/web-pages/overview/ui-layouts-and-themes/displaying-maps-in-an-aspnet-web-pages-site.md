---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Exibição de mapas em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: tfitzmac
description: Este artigo explica como exibir mapas interativos em páginas em um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, Ma...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 4c64791a77f2a72c227ce74e796340c6d8c7316c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812776"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Exibição de mapas em um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como exibir mapas interativos em páginas em um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, MapQuest e Yahoo.
> 
> O que você aprenderá:
> 
> - Como gerar um mapa com base em um endereço.
> - Como gerar um mapa com base nas coordenadas de latitude e longitude.
> - Como registrar uma conta de desenvolvedor do Bing Maps e obter uma chave para uso com o Bing Maps.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O `Maps` auxiliar.
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


Em páginas da Web, você pode exibir mapas em uma página usando `Maps` auxiliar. Você pode gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude. O `Maps` classe permite que você chame em mecanismos de mapa populares, incluindo Bing, Google, MapQuest e Yahoo.

As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar. Basta adicionar uma referência de arquivo JavaScript que faz os métodos disponíveis para exibir o mapa e, em seguida, chamar métodos do `Maps` auxiliar.

Um serviço de mapa com base no qual você escolha `Maps` método auxiliar que você usar. Você pode usar qualquer um destes:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalando as partes que necessárias

Para exibir mapas, você precisa ter essas partes:

- O `Maps` auxiliar. Esse auxiliar está na versão 2 do ASP.NET Web Helpers Library. Se você ainda não adicionou a biblioteca, você pode instalá-lo em seu site como um pacote do NuGet. Para obter detalhes, consulte [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). (Na galeria, pesquise o `microsoft-web-helpers` pacote.)
- A biblioteca jQuery. Vários dos modelos de site do WebMatrix já incluem bibliotecas do jQuery em sua *Script* pastas. Se você não tiver essas bibliotecas, você pode baixar a biblioteca mais recente de jQuery diretamente a partir de [jQuery.org](http://jQuery.org) site. Ou você pode criar um novo site usando um modelo (por exemplo, o **Starter Site** modelo) e, em seguida, copie os arquivos de jQuery desse site para seu site atual.

Por fim, se você quiser usar o Bing maps, você deve primeiro criar uma conta (gratuita) e obter uma chave. Para obter uma chave, siga estas etapas:

1. Criar uma conta de [conta de desenvolvedor do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Você deve ter uma conta da Microsoft (Windows Live ID) também.

    Você pode especificar que você deseja usar a chave para **avaliação e teste**. Se você estiver testando a função de mapeamento no seu próprio computador usando o WebMatrix e o IIS Express, acesse o **Site** espaço de trabalho e anote a URL do seu site (por exemplo, `http://localhost:50408`, embora o número da porta provavelmente será diferente). Você pode usá-lo *localhost* endereço como o site quando você se registra.
2. Depois de registrar para uma conta, vá para o Bing Maps Account Center e clique em **criar ou exibir as chaves**:

    ![mapeamento de-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Registre a chave do Bing cria.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Criar um mapa com base em um endereço (usando o Google)

O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço. Este exemplo mostra como usar o Google Maps.

1. Crie um arquivo chamado *MapAddress.cshtml* na raiz do site. Nesta página irá gerar um mapa com base em um endereço que você passa para ele.
2. Copie o código a seguir para o arquivo, substituindo o conteúdo existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Observe os seguintes recursos da página:

    - O `<script>` elemento no `<head>` elemento. No exemplo, o `<script>` referências de elemento de *jquery 1.6.4.min.js* arquivo, que é uma versão minificada (compactada) da biblioteca jQuery, versão 1.6.4. Observe que a referência supõe que o *. js* arquivo está sendo a *Scripts* pasta do site. 

        > [!NOTE]
        > Se você estiver usando uma versão diferente da biblioteca jQuery, apenas certifique-se de que você está apontando para a mesma versão corretamente.
    - A chamada para o `@Maps.GetGoogleHtml` no corpo da página. Para mapear um endereço, você deve passar uma cadeia de caracteres de endereço. Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Execute a página e digite um endereço. A página exibe um mapa, com base em Google Maps, que mostra o local que você especificou.

     ![mapeamento de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Criar um mapa com base em Latitude e Longitude coordena (usando o Bing)

Este exemplo mostra como criar um mapa com base nas coordenadas. Este exemplo mostra como usar o Bing maps e como incluir sua chave do Bing. (Você pode criar um mapa com base nas coordenadas usando outros mecanismos de mapa também, sem usar uma chave do Bing).

1. Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site e substitua o conteúdo existente com o código e marcação a seguir:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Substitua `your-key-here` com a chave do Bing mapas que você gerou anteriormente.
3. Execute o *MapCoordinates.cshtml* página, insira as coordenadas de latitude e longitude e, em seguida, clique no **mapa-lo!** . (Se você não souber nenhum coordenadas, tente o seguinte. Esse é um local no campus da Microsoft Redmond.)

   - Latitude: 47.6781005859375
   - Longitude:-122.158317565918

     A página é exibida usando as coordenadas que você especificou.

     ![mapeamento de-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Referência da API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
