---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Ciclo de vida de um aplicativo ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Baixe um documento PDF que gráficos o ciclo de vida de um aplicativo ASP.NET MVC 5. Este documento de ciclo de vida fornece uma visão geral do ciclo de vida do MVC um...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833047"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Ciclo de vida de um aplicativo ASP.NET MVC 5
====================
por [Cephas Lin](https://github.com/cephalin)

[Baixe o documento PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Aqui você pode baixar um documento PDF que gráficos o ciclo de vida de todos os aplicativos ASP.NET MVC 5, de HTTP de receber a solicitação para enviar a resposta HTTP de volta ao cliente. Ele foi projetado como uma ferramenta educacional para aqueles que são novos para o ASP.NET MVC e também como uma referência para aqueles que precisem de tantos aspectos específicos do aplicativo. O documento PDF tem os seguintes recursos:

- Relevante [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) estágios para ajudar você a entender onde MVC integra-se a [ciclo de vida de aplicativos ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Uma visão geral do ciclo de vida de aplicativo MVC, onde você possa entender os estágios principais que todos os aplicativos MVC passa por meio do pipeline de processamento de solicitação.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Uma exibição de detalhes que mostra os exercícios para baixo em detalhes sobre o pipeline de processamento de solicitação. Você pode comparar o modo de exibição de alto nível e a exibição de detalhes para ver como os detalhes de ciclos de vida são coletados em vários estágios. [Baixar PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver uma exibição ampliada.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- O posicionamento e a finalidade de todos os métodos substituíveis na [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objeto no pipeline de processamento de solicitação. Você pode ou não ter a necessidade de substituir qualquer método de um, mas é importante para você entender sua função no ciclo de vida do aplicativo para que você possa escrever o código no estágio do ciclo de vida apropriado para o efeito que pretende.
- Diagramas de cima queimado mostrando como cada um dos tipos de filtro (autenticação, autorização, ação e resultado) é invocada.
- Vincular a um artigo útil ou o blog de cada ponto de interesse na exibição detalhes.


## <a name="next-steps"></a>Próximas etapas

Este documento atende às suas necessidades? Agradecemos seus comentários. Se você tiver alguma pergunta sobre o ciclo de vida do ASP.NET MVC em seu aplicativo, [Stackoverflow](http://stackoverflow.com/help) e o [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são ótimos lugares para perguntar. Siga [me](https://twitter.com/Cephas_MSFT) no twitter, para que você possa obter atualizações em meu tutoriais mais recentes.
