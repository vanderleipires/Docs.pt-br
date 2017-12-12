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
ms.openlocfilehash: a802e4fed81fadca195c8aa83c37c7100ac5cbec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Exibição de mapas em um Site de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como exibir mapas interativos em páginas de um site de páginas da Web do ASP.NET (Razor) com base no mapeamento de serviços fornecidos pelo Bing, Google, MapQuest e Yahoo.
> 
> O que você aprenderá:
> 
> - Como gerar um mapa com base em um endereço.
> - Como gerar um mapa com base em coordenadas de latitude e longitude.
> - Como registrar uma conta de desenvolvedor do Bing Maps e obter uma chave para usar com o Bing Maps.
> 
> Este é o recurso ASP.NET introduzido no artigo:
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


Em páginas da Web, você pode exibir mapas em uma página usando `Maps` auxiliar. Você pode gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude. O `Maps` classe permite que você chama mecanismos de mapa populares, incluindo Yahoo, Google, MapQuest e Bing.

As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar. Adicione uma referência de arquivo JavaScript que faz com que os métodos disponíveis para exibir o mapa e, em seguida, você pode chamar métodos do `Maps` auxiliar.

Escolha um serviço de mapa com base no qual `Maps` método auxiliar usado. Você pode usar um dos seguintes:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalar as partes que necessárias

Para exibir mapas, você precisa estas partes:

- O `Maps` auxiliar. Este auxiliar é a versão 2 do ASP.NET Web Helpers Library. Se você ainda não adicionou a biblioteca, você pode instalá-lo em seu site como um pacote do NuGet. Para obter detalhes, consulte [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372). (Na galeria, procure o `microsoft-web-helpers` pacote.)
- A biblioteca jQuery. Vários dos modelos de site do WebMatrix já incluem bibliotecas jQuery em seus *Script* pastas. Se você não tiver essas bibliotecas, você pode baixar a biblioteca mais recente do jQuery diretamente a partir de [jQuery.org](http://jQuery.org) site. Ou você pode criar um novo site usando um modelo (por exemplo, o **Site inicial** modelo) e, em seguida, copie os arquivos de jQuery do site para o site atual.

Finalmente, se você quiser usar o Bing maps, você deve primeiro criar uma conta (gratuita) e obter uma chave. Para obter uma chave, siga estas etapas:

1. Criar uma conta de [conta de desenvolvedor do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Você deve ter uma conta da Microsoft (Windows Live ID) também.

    Você pode especificar que você deseja usar a chave para **avaliação e teste**. Se você estiver testando a função de mapeamento no seu computador usando o WebMatrix e o IIS Express, visite o **Site** espaço de trabalho e anote a URL do seu site (por exemplo, `http://localhost:50408`, embora o número de porta provavelmente será diferente). Você pode usar isso *localhost* endereço do site quando você registrar.
2. Depois que você registrou para uma conta, vá para o Centro de contas do Bing Maps e clique em **criar ou exibir chaves**:

    ![mapeamento de 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Registre a chave que cria Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Criar um mapa com base em um endereço (usando o Google)

O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço. Este exemplo mostra como usar o Google Maps.

1. Crie um arquivo chamado *MapAddress.cshtml* na raiz do site. Esta página irá gerar um mapa com base em um endereço que você passa para ele.
2. Copie o código a seguir para o arquivo, substituindo o conteúdo existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Observe os seguintes recursos da página:

    - O `<script>` elemento o `<head>` elemento. No exemplo, o `<script>` referências de elementos de *1.6.4.min.js jquery* arquivo, que é uma versão minimizada (compactada) da biblioteca do jQuery, versão 1.6.4. Observe que a referência supõe que o *. js* arquivo está no *Scripts* pasta do site. 

        > [!NOTE]
        > Se você estiver usando uma versão diferente da biblioteca jQuery, apenas certifique-se de que você está apontando para a versão corretamente.
    - A chamada para o `@Maps.GetGoogleHtml` no corpo da página. Para mapear um endereço, você deve passar uma cadeia de caracteres do endereço. Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
- Execute a página e digite um endereço. A página exibe um mapa, com base no Google Maps, que mostra o local especificado.

    ![mapeamento de-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Criar um mapa com base em Latitude e Longitude coordenadas (usando o Bing)

Este exemplo mostra como criar um mapa com base nas coordenadas. Este exemplo mostra como usar o Bing maps e como incluir a chave do Bing. (Você pode criar um mapa com base nas coordenadas usando mecanismos de mapa Além disso, sem usar uma chave do Bing).

1. Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site e substituir o conteúdo existente com o seguinte código e marcação:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Substituir `your-key-here` com a chave do Bing Maps que você gerou anteriormente.
3. Execute o *MapCoordinates.cshtml* página, insira as coordenadas de latitude e longitude e, em seguida, clique no **mapa ele!** . (Se você não souber qualquer coordenadas, tente o seguinte. Esse é um local no campus da Microsoft Redmond.)

    - Latitude: 47.6781005859375
    - Longitude:-122.158317565918

    A página é exibida usando as coordenadas especificadas.

    ![mapeamento de-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Referência da API Microsoft.Maps](https://msdn.microsoft.com/en-us/library/gg427611.aspx)
