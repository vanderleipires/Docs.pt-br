---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de um aplicativo ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Baixe um documento PDF que gráficos de ciclo de vida de um aplicativo ASP.NET MVC 5. Este documento de ciclo de vida fornece uma visão geral do ciclo de vida do MVC um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 50d58d10c11677fa72ede6a03e686cbde4cbae1d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036486"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de um aplicativo ASP.NET MVC 5
====================
por [Cephas Lin](https://github.com/cephalin)

[Baixe o documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aqui você pode baixar um documento PDF que gráficos de solicitação de ciclo de vida de todos os aplicativos ASP.NET MVC 5, receba o HTTP para enviar a resposta HTTP de volta ao cliente. Ele foi projetado como uma ferramenta educacional para aqueles que são novos para o ASP.NET MVC e também como uma referência para aqueles que precisam analisar aspectos específicos do aplicativo. O documento PDF tem os seguintes recursos:

- Relevantes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) estágios para ajudá-lo a entender onde MVC integra o [ciclo de vida de aplicativos ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Uma exibição de alto nível do ciclo de vida do aplicativo MVC, em que você possa entender as etapas principais que cada aplicativo MVC passa por meio do pipeline de processamento de solicitação.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Uma exibição de detalhes que mostra detalhada para baixo em detalhes sobre o pipeline de processamento de solicitação. Você pode comparar o modo de exibição de alto nível e a exibição de detalhes para ver como os detalhes de ciclos de vida são coletados em várias fases. [Baixar PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver uma exibição maior.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- O posicionamento e a finalidade de todos os métodos substituíveis no [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objeto no pipeline de processamento de solicitação. Você pode ou não ter a necessidade de substituir qualquer método de um, mas é importante entender sua função no ciclo de vida do aplicativo para que você pode escrever o código no estágio do ciclo de vida apropriado para o efeito que pretende.
- Diagramas de queimado-up mostrando como cada um dos tipos de filtro (autenticação, autorização, ação e resultado) é invocada.
- Link para um artigo útil ou blog de cada ponto de interesse na exibição detalhes.


## <a name="next-steps"></a>Próximas etapas

Este documento está de acordo com sua necessidade? Apreciamos seus comentários. Se você tiver alguma pergunta sobre o ciclo de vida do ASP.NET MVC no seu aplicativo, [Stackoverflow](http://stackoverflow.com/help) e [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para solicitar. Execute [me](https://twitter.com/Cephas_MSFT) no twitter para que você possa obter atualizações em meu tutoriais mais recentes.
