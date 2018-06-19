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
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a><span data-ttu-id="8ce06-104">Ciclo de vida de um aplicativo ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="8ce06-104">Lifecycle of an ASP.NET MVC 5 Application</span></span>
====================
<span data-ttu-id="8ce06-105">por [Cephas Lin](https://github.com/cephalin)</span><span class="sxs-lookup"><span data-stu-id="8ce06-105">by [Cephas Lin](https://github.com/cephalin)</span></span>

[<span data-ttu-id="8ce06-106">Baixe o documento PDF</span><span class="sxs-lookup"><span data-stu-id="8ce06-106">Download PDF Document</span></span>](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

<span data-ttu-id="8ce06-107">Aqui você pode baixar um documento PDF que gráficos de solicitação de ciclo de vida de todos os aplicativos ASP.NET MVC 5, receba o HTTP para enviar a resposta HTTP de volta ao cliente.</span><span class="sxs-lookup"><span data-stu-id="8ce06-107">Here you can download a PDF document that charts the lifecycle of every ASP.NET MVC 5 application, from receiving the HTTP request to sending the HTTP response back to the client.</span></span> <span data-ttu-id="8ce06-108">Ele foi projetado como uma ferramenta educacional para aqueles que são novos para o ASP.NET MVC e também como uma referência para aqueles que precisam analisar aspectos específicos do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8ce06-108">It is designed both as an educational tool for those who are new to ASP.NET MVC and also as a reference for those who need to drill into specific aspects of the application.</span></span> <span data-ttu-id="8ce06-109">O documento PDF tem os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="8ce06-109">The PDF document has the following features:</span></span>

- <span data-ttu-id="8ce06-110">Relevantes [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) estágios para ajudá-lo a entender onde MVC integra o [ciclo de vida de aplicativos ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ce06-110">Relevant [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) stages to help you understand where MVC integrates into the [ASP.NET application lifecycle](https://msdn.microsoft.com/library/bb470252.aspx).</span></span>
- <span data-ttu-id="8ce06-111">Uma exibição de alto nível do ciclo de vida do aplicativo MVC, em que você possa entender as etapas principais que cada aplicativo MVC passa por meio do pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8ce06-111">A high-level view of the MVC application lifecycle, where you can understand the major stages that every MVC application passes through in the request processing pipeline.</span></span>  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- <span data-ttu-id="8ce06-112">Uma exibição de detalhes que mostra detalhada para baixo em detalhes sobre o pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8ce06-112">A detail view that shows drills down into the details of the request processing pipeline.</span></span> <span data-ttu-id="8ce06-113">Você pode comparar o modo de exibição de alto nível e a exibição de detalhes para ver como os detalhes de ciclos de vida são coletados em várias fases.</span><span class="sxs-lookup"><span data-stu-id="8ce06-113">You can compare the high-level view and the detail view to see how the lifecycles details are collected into the various stages.</span></span> <span data-ttu-id="8ce06-114">[Baixar PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) para ver uma exibição maior.</span><span class="sxs-lookup"><span data-stu-id="8ce06-114">[Download PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) to see a larger view.</span></span>
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- <span data-ttu-id="8ce06-115">O posicionamento e a finalidade de todos os métodos substituíveis no [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) objeto no pipeline de processamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8ce06-115">Placement and purpose of all overridable methods on the [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) object in the request processing pipeline.</span></span> <span data-ttu-id="8ce06-116">Você pode ou não ter a necessidade de substituir qualquer método de um, mas é importante entender sua função no ciclo de vida do aplicativo para que você pode escrever o código no estágio do ciclo de vida apropriado para o efeito que pretende.</span><span class="sxs-lookup"><span data-stu-id="8ce06-116">You may or may not have the need to override any one method, but it is important for you to understand their role in the application lifecycle so that you can write code at the appropriate life cycle stage for the effect you intend.</span></span>
- <span data-ttu-id="8ce06-117">Diagramas de queimado-up mostrando como cada um dos tipos de filtro (autenticação, autorização, ação e resultado) é invocada.</span><span class="sxs-lookup"><span data-stu-id="8ce06-117">Blown-up diagrams showing how each of the filter types (authentication, authorization, action, and result) is invoked.</span></span>
- <span data-ttu-id="8ce06-118">Link para um artigo útil ou blog de cada ponto de interesse na exibição detalhes.</span><span class="sxs-lookup"><span data-stu-id="8ce06-118">Link to a useful article or blog from each point of interest in the detail view.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8ce06-119">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="8ce06-119">Next Steps</span></span>

<span data-ttu-id="8ce06-120">Este documento está de acordo com sua necessidade?</span><span class="sxs-lookup"><span data-stu-id="8ce06-120">Does this document meet your need?</span></span> <span data-ttu-id="8ce06-121">Apreciamos seus comentários.</span><span class="sxs-lookup"><span data-stu-id="8ce06-121">We'd appreciate your feedback.</span></span> <span data-ttu-id="8ce06-122">Se você tiver alguma pergunta sobre o ciclo de vida do ASP.NET MVC no seu aplicativo, [Stackoverflow](http://stackoverflow.com/help) e [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para solicitar.</span><span class="sxs-lookup"><span data-stu-id="8ce06-122">If you have any question on the ASP.NET MVC lifecycle in your application, [Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are great places to ask.</span></span> <span data-ttu-id="8ce06-123">Execute [me](https://twitter.com/Cephas_MSFT) no twitter para que você possa obter atualizações em meu tutoriais mais recentes.</span><span class="sxs-lookup"><span data-stu-id="8ce06-123">Follow [me](https://twitter.com/Cephas_MSFT) on twitter so you can get updates on my latest tutorials.</span></span>
