---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderização da Web do ASP.NET (Razor) Sites de páginas para dispositivos móveis | Microsoft Docs
author: tfitzmac
description: 'Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que será renderizado corretamente em dispositivos móveis. O que você vai aprender: como você...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 7159283efcd4a82d5dff68f2d9e315f804e7cdcb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831116"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Renderização ASP.NET Web Pages (Razor) Sites para dispositivos móveis
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar páginas em um site de páginas da Web do ASP.NET (Razor) que será renderizado corretamente em dispositivos móveis.
> 
> O que você aprenderá:
> 
> - Como usar uma convenção de nomenclatura para especificar que uma página foi projetado especificamente para dispositivos móveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


Páginas da Web ASP.NET permite criar exibições personalizadas para processar o conteúdo em celular ou outros dispositivos.

A maneira mais simples para criar a página específica do dispositivo em um site de páginas da Web ASP.NET é usando um padrão de nomenclatura de arquivo como este: <em>FileName.</em> <em>Mobile</em><em>. cshtml</em>. Você pode criar duas versões de uma página (por exemplo, uma nomeada <em>MyFile.cshtml</em> e outro chamado <em>MyFile.Mobile.cshtml</em>). No tempo de execução, quando um dispositivo móvel solicita <em>MyFile.cshtml</em>, o ASP.NET processa o conteúdo de <em>MyFile.Mobile.cshtml</em>. Caso contrário, <em>MyFile.cshtml</em> é renderizado.

O exemplo a seguir mostra como habilitar a renderização móvel com a adição de uma página de conteúdo para dispositivos móveis. *Page1.cshtml* contém conteúdo além de uma barra lateral de navegação. *Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.

1. Em um site de páginas da Web ASP.NET, crie um arquivo chamado *Page1.cshtml* e substitua o conteúdo atual pela seguinte marcação.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Crie um arquivo chamado *Page1.Mobile.cshtml* e substitua o conteúdo existente com a marcação a seguir. Observe que a versão móvel da página omite a seção de navegação para a renderização de melhor em uma tela menor.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Execute um navegador de desktop e navegue até *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Execute um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*. (Observe que você não incluir *Mobile.* como parte da URL). Mesmo que a solicitação é para *Page1.cshtml*, o ASP.NET processa *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Para testar páginas para dispositivos móveis, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop. Essa ferramenta lhe permite testar páginas da web conforme eles ficaria em dispositivos móveis (isto é, normalmente com muito menor área de exibição). Um exemplo de um simulador de é o [complemento de alternador de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para o Mozilla Firefox, que lhe permite emular vários navegadores para dispositivos móveis de uma versão da área de trabalho do Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Emulador do Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
