---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Informações de visitante (análise) do controle de uma ASP.NET Web Pages (Razor) Site | Microsoft Docs"
author: tfitzmac
description: "Depois que você tiver chegado a seu site for, você talvez queira analisar o tráfego do site."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Informações de controle visitante (análise) de um Site ASP.NET da páginas (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como usar um auxiliar para adicionar a análise de site para páginas em um site de páginas da Web do ASP.NET (Razor).
> 
> O que você aprenderá:
> 
> - Como enviar informações sobre o tráfego do site para um provedor de análise.
> 
> Esses são o ASP.NET recursos introduzidos no artigo de programação:
> 
> - O `Analytics` auxiliar.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - ASP.NET Web Helpers Library (pacote do NuGet)


A análise é um termo geral para a tecnologia que mede o tráfego em seu site para que você possa entender como as pessoas usam o site. Muitos serviços de análise estão disponíveis, incluindo os serviços do Google, Yahoo, StatCounter e outros.

A análise de maneira funciona é que você se inscrever para uma conta com o provedor de análise, em que você registrar o site que você deseja rastrear. O provedor envia um trecho de código JavaScript que inclui uma ID ou o controle de código para sua conta. Você pode adicionar o trecho de JavaScript para as páginas da web no site que você deseja controlar. (Geralmente você adiciona o trecho de análise para uma página de layout ou de rodapé ou outra marcação HTML que aparece em cada página em seu site.) Quando os usuários solicitam uma página que contém um dos trechos de código JavaScript, o trecho envia informações sobre a página atual para o provedor de análise, que registra vários detalhes sobre a página.

Quando você deseja examinar as estatísticas de site, você faça logon no site do provedor de análise. Você pode exibir todos os tipos de relatórios sobre o seu site, como:

- O número de modos de exibição de página para páginas individuais. Isso lhe permite (aproximadamente) quantas pessoas estão visitando o site e as páginas em seu site são mais populares.
- Quanto tempo as pessoas gastam em páginas específicas. Isso pode lhe coisas como se a home page é manter os juros de pessoas.
- O que as pessoas de sites estavam antes de eles visitam o site. Isso ajuda você a entender se o tráfego é proveniente de links de pesquisas e assim por diante.
- Quando as pessoas visitam seu site e quanto tempo eles permanecem.
- Quais países os visitantes são do.
- Quais sistemas operacionais e navegadores os visitantes estão usando.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Usando um auxiliar para adicionar análise para uma página

Páginas da Web ASP.NET inclui vários auxiliares de análise (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) que tornam mais fácil de gerenciar os trechos de código JavaScript usados para análise. Em vez de descobrir como e onde colocar o código JavaScript, você precisa fazer é adicionar o auxiliar a uma página. A única informação que você precisa fornecer é o nome da conta, a ID ou o código de rastreamento. (Para StatCounter, você também precisa fornecer alguns valores adicionais.)

Neste procedimento, você criará uma página de layout que usa o `GetGoogleHtml` auxiliar. Se você já tiver uma conta com um dos provedores de análise, você pode usar a conta e fazer pequenas ajustes conforme necessário.

> [!NOTE]
> Quando você cria uma conta de análise, você pode registrar a URL do site que você deseja ser controle. Se você estiver testando tudo no computador local, você não controle tráfego real (somente do tráfego é você), portanto, não será capaz de registro e exibir estatísticas de site. Mas, este procedimento mostra como adicionar um auxiliar de análise para uma página. Quando você publica seu site, o site enviará informações para seu provedor de análise.


1. Adicionar o ASP.NET Web Helpers Library ao seu site, conforme descrito em [auxiliares instalando em um Site de páginas da Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não tenha adicionado.
2. Criar uma conta com o Google Analytics e registre o nome da conta.
3. Crie uma página de layout chamada *Analytics.cshtml* e adicione a seguinte marcação:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Você deve colocar a chamada para o `Analytics` auxiliar no corpo da página da web (antes do `</body>` marca). Caso contrário, o navegador não pode executar o script.

    Se você estiver usando um provedor de análise diferente, use um dos seguintes auxiliares:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Substituir `myaccount` com o nome da conta, o ID ou o código de controle que você criou na etapa 1.
5. Execute a página no navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.)
6. No navegador, exiba a origem da página. Você poderá ver o código de análise renderizado:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Faça logon no site do Google Analytics e examinar as estatísticas para o seu site. Se você estiver executando a página em um site ao vivo, você verá uma entrada que registra a visita para sua página.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Site do Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Análise site da Web](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Site de StatCounter](http://statcounter.com/)
