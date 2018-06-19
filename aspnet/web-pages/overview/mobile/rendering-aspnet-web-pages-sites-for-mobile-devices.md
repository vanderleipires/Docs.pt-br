---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Processamento da Web do ASP.NET páginas de Sites (Razor) para dispositivos móveis | Microsoft Docs
author: tfitzmac
description: 'Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que serão renderizados corretamente em dispositivos móveis. Você aprenderá: como você...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897313"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderização de páginas (Razor) Sites do ASP.NET para dispositivos móveis
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que serão renderizados corretamente em dispositivos móveis.
> 
> O que você aprenderá:
> 
> - Como usar uma convenção de nomenclatura para especificar que uma página foi desenvolvido especificamente para dispositivos móveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


Páginas da Web ASP.NET permite criar exibições personalizadas para processar o conteúdo no celular ou outros dispositivos.

É a maneira mais simples para criar a página específica do dispositivo em um site de páginas da Web ASP.NET usando um padrão de nomenclatura de arquivo como este: <em>FileName.</em> <em>Mobile</em><em>. cshtml</em>. Você pode criar duas versões de uma página (por exemplo, um denominado <em>MyFile.cshtml</em> e um chamado <em>MyFile.Mobile.cshtml</em>). No tempo de execução, quando um dispositivo móvel solicita <em>MyFile.cshtml</em>, ASP.NET processa o conteúdo de <em>MyFile.Mobile.cshtml</em>. Caso contrário, <em>MyFile.cshtml</em> é renderizado.

O exemplo a seguir mostra como habilitar renderização móvel com a adição de uma página de conteúdo para dispositivos móveis. *Page1.cshtml* contém conteúdo mais de uma barra lateral de navegação. *Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.

1. Em um site de páginas da Web ASP.NET, crie um arquivo chamado *Page1.cshtml* e substitua o conteúdo atual pela seguinte marcação.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Crie um arquivo chamado *Page1.Mobile.cshtml* e substitua o conteúdo existente com a seguinte marcação. Observe que a versão móvel da página omite a seção de navegação para renderização melhor em uma tela menor.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Executar um navegador da área de trabalho e navegue até *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Executar um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*. (Observe que você não incluir *.mobile.* como parte da URL). Mesmo que a solicitação é para *Page1.cshtml*, o ASP.NET processa *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Para testar as páginas de celular, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop. Essa ferramenta permite que você teste páginas da web, como seria aparecem em dispositivos móveis (isto é, normalmente com muito menor Exibir área). É um exemplo de um simulador de [complemento alternador de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite que você emular vários navegadores móveis de uma versão de área de trabalho do Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Emulador do Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
