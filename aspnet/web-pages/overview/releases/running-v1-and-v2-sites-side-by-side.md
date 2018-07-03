---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Executando versões diferentes do ASP.NET Web Pages (Razor) lado a lado | Microsoft Docs
author: tfitzmac
description: Este artigo explica como executar o ASP.NET Web Pages (Razor) sites no mesmo computador ou servidor quando os sites são configurados para usar diferentes versões...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: aa2bf41054540cc67b7c0b4669e356e732fed8d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402451"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Executando versões diferentes do ASP.NET Web Pages (Razor) lado a lado
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como executar o ASP.NET Web Pages (Razor) sites no mesmo computador ou servidor quando os sites são configurados para usar versões diferentes de páginas da Web do ASP.NET.
> 
> O que você aprenderá:
> 
> - O que o comportamento padrão é no ASP.NET quando você tiver sites criados com páginas da Web do ASP.NET.
> - Como configurar um novo site para ser executado com uma versão mais antiga de páginas da Web do ASP.NET.
>   
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O `webPages:Version` definição de configuração.
>   
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2 e páginas da Web de ASP.NET 1.0.


Páginas da Web do ASP.NET oferece suporte à capacidade de executar sites lado a lado. Isso permite que você continue a executar seus aplicativos mais antigos de páginas da Web ASP.NET, criar novos aplicativos de páginas da Web ASP.NET e executar todas elas no mesmo computador.

Aqui estão algumas coisas a lembrar quando você instala as páginas da Web com o WebMatrix:

- Por padrão, os aplicativos existentes de páginas da Web serão executado como a versão mais recente em seu computador. (Os assemblies são instalados no cache de assembly global (GAC) e são usados automaticamente.)
- Se você quiser executar um site usando uma versão diferente de páginas da Web do ASP.NET, você pode configurar o site para fazer isso. Se seu site ainda não tiver um *Web. config* de arquivo na raiz do site, crie um novo e copie o XML a seguir para ele, substituindo o conteúdo existente. Se o site já contém um *Web. config* do arquivo, adicione uma `<appSettings>` elemento como a seguir para o `<configuration>` seção.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-Se você não especificar uma versão na *Web. config* arquivo, um site é implantado como a versão mais recente. (Os assemblies são copiados para o *bin* pasta no site implantado.)
- Novos aplicativos que você cria usando os modelos de site na Web Matrix incluem os assemblies da versão de páginas da Web do site *bin* pasta.

Em geral, você sempre pode controlar qual versão das páginas da Web para usar com seu site usando o NuGet para instalar os assemblies apropriados para o site *bin* pasta. Para encontrar pacotes, visite [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Recursos adicionais

[Os principais recursos do ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
