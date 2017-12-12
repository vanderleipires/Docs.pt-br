---
uid: webhooks/source
title: "ASP.NET WebHooks código-fonte e pacotes do NuGet | Microsoft Docs"
author: rick-anderson
description: "Links para ASP.NET WebHooks código-fonte e pacotes do NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="57c8c-103">ASP.NET WebHooks código-fonte e pacotes do NuGet</span><span class="sxs-lookup"><span data-stu-id="57c8c-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="57c8c-104">Microsoft ASP.NET WebHooks faz parte da família Microsoft ASP.NET de módulos e está hospedado como um [Abrir projeto de origem no GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="57c8c-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="57c8c-105">Isso significa que podemos aceitar contribuições, mas examine o [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação pull.</span><span class="sxs-lookup"><span data-stu-id="57c8c-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="57c8c-106">Esta documentação online que você está lendo agora também está hospedada como [código-fonte aberto no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.</span><span class="sxs-lookup"><span data-stu-id="57c8c-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="57c8c-107">Pacotes do NuGet</span><span class="sxs-lookup"><span data-stu-id="57c8c-107">Nuget Packages</span></span>

<span data-ttu-id="57c8c-108">Microsoft ASP.NET WebHooks também está disponível como visualização pacotes Nuget, que significa que você precisa selecionar o sinalizador de visualização no Visual Studio para vê-los.</span><span class="sxs-lookup"><span data-stu-id="57c8c-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="57c8c-109">O [pacotes do Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são devided em três partes:</span><span class="sxs-lookup"><span data-stu-id="57c8c-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="57c8c-110">[Comuns](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): um pacote comum que é compartilhado entre remetentes e destinatários.</span><span class="sxs-lookup"><span data-stu-id="57c8c-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="57c8c-111">[Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): um conjunto de pacotes com suporte enviar seus próprios WebHooks a outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="57c8c-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="57c8c-112">A funcionalidade para enviar WebHooks é descrita mais detalhadamente na [WebHooks enviar](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="57c8c-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="57c8c-113">[Receptores de](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): um conjunto de pacotes com suporte receber WebHooks de outros.</span><span class="sxs-lookup"><span data-stu-id="57c8c-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="57c8c-114">A funcionalidade para receber WebHooks é descrita mais detalhadamente na [WebHooks recebendo](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="57c8c-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
