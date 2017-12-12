---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "Cache de dados em uma Web ASP.NET páginas Site (Razor) para melhorar o desempenho | Microsoft Docs"
author: tfitzmac
description: "Você pode acelerar o seu site por fazer com que ele armazenar - ou seja, cache - os resultados de dados que normalmente seriam levar um tempo considerável para recuperar ou processar um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: c747fef33a6d1db19f09fd0303c47d689b956687
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Cache de dados em um Site de páginas (Razor) da Web do ASP.NET para melhorar o desempenho
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um auxiliar de informações em cache para melhorar o desempenho em um site de páginas da Web do ASP.NET (Razor). Você pode acelerar o seu site fazendo armazenamento &#8212; ou seja, cache &#8212; os resultados de dados que normalmente seria levar um tempo considerável para recuperar ou processar e que não são alterados com frequência.
> 
> **O que você aprenderá:** 
> 
> - Como usar o cache para melhorar a capacidade de resposta do seu site.
> 
> Estes são os recursos ASP.NET introduzidos no artigo:
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
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


Sempre que alguém solicita uma página do seu site, o servidor web deve realizar algum trabalho para atender à solicitação. Para algumas das suas páginas, o servidor pode ter que executar tarefas que demoram (comparativamente), como recuperar dados de um banco de dados. Mesmo que essas tarefas não têm tempo em termos absolutos, se seu site tiver muito tráfego, uma série inteira de solicitações individuais que fazer com que o servidor web executar a tarefa complexa ou lenta pode se acumular para uma grande parte do trabalho. Por fim, isso pode afetar o desempenho do site.

É uma maneira de melhorar o desempenho do seu site em circunstâncias assim os dados em cache. Se seu site obtém repetidas solicitações para as mesmas informações e as informações não precisam ser modificada para cada pessoa e não é tempo confidenciais, em vez de buscar novamente ou recalcular, poderá buscar os dados uma vez e, em seguida, armazenar os resultados. Na próxima vez que entrar uma solicitação para que informações, você simplesmente obtê-lo no cache.

Em geral, você pode armazenar informações que não são alterados com frequência. Quando você coloca informações no cache, ele é armazenado na memória no servidor web. Você pode especificar quanto tempo ele deve ser armazenado em cache, de segundos a dias. Quando o período de cache expira, as informações são automaticamente removidas do cache.

> [!NOTE]
> Entradas no cache podem ser removidas por razões diferentes de que elas tiverem expirado. Por exemplo, o servidor web pode temporariamente esgotamento da memória e uma maneira pode recuperar memória é gerar entradas do cache do. Como você verá, mesmo se você tiver colocado informações no cache, você precisa Certifique-se de que ele ainda está quando precisar dele.


Imagine que o site tiver uma página que exibe a temperatura atual e a previsão do tempo. Para obter esse tipo de informação, você pode enviar uma solicitação para um serviço externo. Como essas informações não mudam muito (dentro de um período de tempo de duas horas, por exemplo) e como chamadas externas exigem tempo e largura de banda, é um bom candidato para o cache.

## <a name="adding-caching-to-a-page"></a>Adicionando o cache para uma página

O ASP.NET inclui um `WebCache` auxiliar que torna mais fácil adicionar cache ao seu site e adicionar dados ao cache. Neste procedimento, você criará uma página que armazena a hora atual. Isso não é um exemplo do mundo real, como a hora atual é algo que são alterados com frequência e que Além disso não é complexa para ser calculada. No entanto, é uma boa maneira de ilustrar cache em ação.

1. Adicionar uma nova página chamada *WebCache.cshtml* para o site.
2. Adicione o seguinte código e marcação para a página:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Quando os dados armazenados em cache, você colocá-lo em cache usando um nome isso é exclusivo em todo o site. Nesse caso, você usará uma entrada de cache nomeada `CachedTime`. Este é o `cacheItemKey` mostrado no exemplo de código.

    O código primeiro lê o `CachedTime` entrada de cache. Se um valor é retornado (ou seja, se a entrada de cache não é nulo), o código define apenas o valor da variável de tempo para o cache de dados.

    No entanto, se a entrada de cache não existe (ou seja, é nulo), o código define o valor de tempo, adiciona-o ao cache e define um valor de expiração de um minuto. Após um minuto, a entrada de cache é descartada. (O valor de expiração padrão para um item no cache é de 20 minutos.) O comando `WebCache.Set(cacheItemKey, time, 1, false)` mostra como adicionar o valor de tempo atual para o cache e sua validade do conjunto de 1 minuto. Definindo o *slidingExpiration* parâmetro `false` significa que o tempo de expiração não for renovado a cada vez que forem solicitados. Ela expirará exatamente 1 minuto depois que ele foi originalmente adicionado ao cache. Se você definir esse valor como `true` o tempo de expiração de 1 minuto é redefinido cada vez que o valor é solicitado do cache.

    Esse código ilustra o padrão que você sempre deve usar quando você armazena dados em cache. Antes de você obterá algo fora do cache, sempre verifique primeiro se o `WebCache.Get` método retornou nulo. Lembre-se de que a entrada de cache pode ter expirado ou pode ter sido removida por algum outro motivo, qualquer entrada fornecida nunca é garantida em cache.
3. Executar *WebCache.cshtml* em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.) Na primeira vez que você solicitar a página, os dados de tempo não estão no cache e o código deve adicionar o valor de tempo para o cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Atualizar *WebCache.cshtml* no navegador. Neste momento, os dados de hora estão no cache. Observe que o tempo não foi alterada desde a última vez que você exibir a página.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Aguarde um minuto para o cache ser esvaziado e, em seguida, atualize a página. A página novamente indica que os dados de tempo não foram encontrados no cache e a hora da atualização é adicionada ao cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


- [Exibindo dados em um gráfico](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Referência da API do WebCache](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
