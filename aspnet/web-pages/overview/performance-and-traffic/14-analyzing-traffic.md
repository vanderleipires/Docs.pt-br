---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Controlando as informações de visitante (análise) para uma Web do ASP.NET (Razor) sites de páginas para | Microsoft Docs
author: Rick-Anderson
description: Depois que você tiver chegado a seu site vai, convém analisar o tráfego do site.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 57e6a0d4681f147faa5e9ca3b6ed0ef287d6a381
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020878"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Controlando as informações de visitante (análise) para um Site do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar um auxiliar para adicionar a análise de sites para páginas em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar informações sobre seu tráfego do site para um provedor de análise.
> 
> Estes são os recursos introduzidos no artigo de programação do ASP.NET:
> 
> - O `Analytics` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - ASP.NET Web Helpers Library (pacote do NuGet)


Análise é um termo geral para a tecnologia que mede o tráfego em seu site para que você possa entender como as pessoas usam o site. Muitos serviços de análise estão disponíveis, incluindo os serviços do Google, Yahoo, StatCounter e outras pessoas.

A análise de maneira que funciona é que você se inscrever para uma conta com o provedor de análise, em que você registre o site que você deseja controlar. O provedor envia um trecho de código JavaScript que inclui uma ID ou código para sua conta de rastreamento. Você pode adicionar o trecho de JavaScript em páginas da web no site que você deseja rastrear. (Você normalmente adiciona o trecho de código de análise a uma página de layout ou de rodapé ou outra marcação HTML que aparece em cada página em seu site.) Quando os usuários solicitam uma página que contém um dos trechos de código JavaScript, o trecho de código envia informações sobre a página atual para o provedor de análise, que registra vários detalhes sobre a página.

Quando você deseja examinar as estatísticas de site, você faça logon no site do provedor de análise. Em seguida, você pode exibir todos os tipos de relatórios sobre seu site, como:

- O número de exibições de página para páginas individuais. Isso informa ao (aproximadamente) quantas pessoas estão visitando o site e quais páginas no seu site são os mais populares.
- Quanto tempo as pessoas gastam em páginas específicas. Isso pode lhe coisas como se sua home page é manter o interesse das pessoas.
- O que as pessoas de sites estavam antes de eles visitam o site. Isso ajuda você a entender se o tráfego é proveniente de links de pesquisas e assim por diante.
- Quando as pessoas visitam seu site e quanto tempo eles permanecem.
- Quais países os visitantes são de.
- Quais navegadores e sistemas operacionais os visitantes estão usando.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Usando um auxiliar para adicionar o Analytics a uma página

Páginas da Web ASP.NET inclui vários auxiliares de análise (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) que facilitam a gerenciar os trechos de código JavaScript usados para análise. Em vez de descobrir como e onde colocar o código JavaScript, tudo o que você precisa fazer é adicionar o auxiliar a uma página. A única informação que você precisa fornecer é o nome da conta, ID ou código de rastreamento. (Para StatCounter, você também precisa fornecer alguns valores adicional.)

Neste procedimento, você criará uma página de layout que usa o `GetGoogleHtml` auxiliar. Se você já tiver uma conta com um dos provedores de análise, você pode usar a conta e fazer alguns pequenos ajustes conforme necessário.

> [!NOTE]
> Quando você cria uma conta da análise, você pode registrar a URL do site que você deseja acompanhar. Se você estiver testando tudo em seu computador local, você não rastreando tráfego real (somente o tráfego é você), portanto, não será capaz de registro e exibir estatísticas de site. Mas esse procedimento mostra como adicionar um auxiliar de análise para uma página. Quando você publica seu site, o site ao vivo enviará informações para seu provedor de análise.


1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares de instalação em um Site de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.
2. Criar uma conta com o Google Analytics e registre o nome da conta.
3. Criar uma página de layout denominada *Analytics.cshtml* e adicione a seguinte marcação:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Você deve colocar a chamada para o `Analytics` auxiliar no corpo da sua página da web (antes do `</body>` marca). Caso contrário, o navegador não executará o script.

    Se você estiver usando um provedor de análise diferente, use um dos seguintes auxiliares:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Substitua `myaccount` com o nome da conta, ID ou código de rastreamento que você criou na etapa 1.
5. Execute a página no navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.)
6. No navegador, exiba a origem da página. Você poderá ver o código de análise renderizado:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Faça logon no site do Google Analytics e examinar as estatísticas para o seu site. Se você estiver executando a página em um site ativo, você pode ver uma entrada que registra a visita à sua página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Site do Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Site do Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Site StatCounter](http://statcounter.com/)
