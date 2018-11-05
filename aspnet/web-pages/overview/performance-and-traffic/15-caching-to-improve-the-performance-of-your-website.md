---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Cache de dados em uma Web do ASP.NET (Razor) sites de páginas para melhorar o desempenho | Microsoft Docs
author: Rick-Anderson
description: Você pode acelerar seu site fazendo com que ele armazenar - ou seja, cache - os resultados de dados que normalmente levaria um tempo considerável para recuperar ou processar um...
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: ede341e02869a9c81cbe2fa7ef97345dc87519a1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020359"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Cache de dados em um Site do ASP.NET Web Pages (Razor) para melhorar o desempenho
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um auxiliar de informações em cache para melhorar o desempenho em um site de páginas da Web do ASP.NET (Razor). Você pode acelerar o seu site, fazendo com que ele armazenar &#8212; ou seja, armazenar em cache &#8212; dos resultados de dados que normalmente levaria um tempo considerável para recuperar ou processar e que não é alterada com frequência.
> 
> **O que você aprenderá:** 
> 
> - Como usar o cache para melhorar a capacidade de resposta do seu site.
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - O `WebCache` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


Sempre que alguém solicita uma página do seu site, o servidor web tem que fazer algum trabalho para atender à solicitação. Para algumas das suas páginas, o servidor talvez precise executar tarefas que demoram (comparativamente), como recuperar dados de um banco de dados. Mesmo que essas tarefas não tirar longo em termos absolutos, se seu site experimentar muito tráfego, uma série inteira de solicitações individuais que fazem com que o servidor web executar a tarefa complicada ou lenta pode adicionar até muito trabalho. Por fim, isso pode afetar o desempenho do site.

Uma maneira de melhorar o desempenho do seu site em circunstâncias como isso é armazenar dados em cache. Se seu site obtém solicitações repetidas para as mesmas informações e as informações não precisam ser modificados para cada pessoa e não é tempo confidenciais, em vez de buscar novamente ou recalcular a ele, você pode buscar os dados uma vez e, em seguida, armazenar os resultados. Na próxima vez que uma solicitação chega para que informações, você simplesmente obtê-lo fora do cache.

Em geral, você armazenar em cache informações que não são alterados com frequência. Quando você coloca informações no cache, ele é armazenado na memória no servidor web. Você pode especificar quanto tempo ele deve ser armazenado em cache, de segundos para dias. Quando expira o período de cache, as informações são automaticamente removidas do cache.

> [!NOTE]
> Entradas no cache podem ser removidas por motivos diferente de que elas tiverem expirado. Por exemplo, o servidor web pode temporariamente esgotamento da memória e uma maneira que ele pode recuperar memória está lançando entradas do cache do. Como você verá, mesmo se você tiver colocado informações no cache, você precisa Certifique-se de que ele ainda está lá quando necessário.


Imagine que seu site tem uma página que exibe a temperatura atual e a previsão do tempo. Para obter esse tipo de informação, você pode enviar uma solicitação para um serviço externo. Como essas informações não mudará muito (dentro de um período de tempo de duas horas, por exemplo) e como chamadas externas exigem tempo e largura de banda, é um bom candidato para armazenar em cache.

## <a name="adding-caching-to-a-page"></a>Adicionando o cache para uma página

O ASP.NET inclui um `WebCache` auxiliar que torna mais fácil adicionar cache ao seu site e adicionar dados ao cache. Neste procedimento, você criará uma página que armazena em cache a hora atual. Isso não é um exemplo do mundo real, como a hora atual é algo que for alterado com frequência e que Além disso não é complexa para calcular. No entanto, é uma boa maneira de ilustrar o cache em funcionamento.

1. Adicionar uma nova página chamada *WebCache.cshtml* para o site.
2. Adicione o seguinte código e marcação à página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Quando você armazenar dados em cache, você colocá-lo no cache usando um nome isso é exclusivo em todo o site. Nesse caso, você usará uma entrada de cache nomeada `CachedTime`. Esse é o `cacheItemKey` mostrado no exemplo de código.

    O código lê primeiro os `CachedTime` entrada de cache. Se um valor é retornado (ou seja, se a entrada de cache não for nula), o código simplesmente define o valor da variável de tempo para os dados do cache.

    No entanto, se a entrada de cache não existe (ou seja, é nulo), o código define o valor de tempo, adiciona-o ao cache e define um valor de expiração para um minuto. Após um minuto, a entrada de cache é descartada. (O valor de expiração padrão para um item em cache é de 20 minutos.) O comando `WebCache.Set(cacheItemKey, time, 1, false)` mostra como adicionar o valor de tempo atual ao cache e definir a expiração para 1 minuto. Definindo o *slidingExpiration* parâmetro `false` significa que o tempo de expiração não for renovado a cada vez que forem solicitados. Ela expirará exatamente 1 minuto depois que ele foi originalmente adicionado ao cache. Se você definir esse valor como `true` o tempo de expiração de 1 minuto é redefinido cada vez que o valor é solicitado do cache.

    Esse código ilustra o padrão que você sempre deve usar quando você armazena dados em cache. Antes de você obtém algo fora do cache, sempre verifique primeiro se o `WebCache.Get` método retornou nulo. Lembre-se de que a entrada de cache pode ter expirado ou pode ter sido removida por algum outro motivo, portanto, qualquer entrada fornecida nunca é garantida para ser no cache.
3. Execute *WebCache.cshtml* em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) Na primeira vez que você solicite a página, os dados de tempo não estiverem no cache, e o código tem de adicionar o valor de tempo para o cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Atualize *WebCache.cshtml* no navegador. Neste momento, os dados de tempo estão no cache. Observe que o tempo não foi alterado desde a última vez em que você exibiu a página.

    ![cache 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Aguarde um minuto para que o cache ser esvaziado e, em seguida, atualize a página. A página novamente indica que os dados de tempo não foi encontrados no cache e a hora da atualização é adicionada ao cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Exibindo dados em um gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referência da API do WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
