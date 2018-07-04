---
uid: webhooks/source
title: Código-fonte WebHooks do ASP.NET e pacotes do NuGet | Microsoft Docs
author: rick-anderson
description: Links para código-fonte WebHooks do ASP.NET e pacotes do NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.openlocfilehash: 49a6d3e92e8d6365bea6594a616922aff9d0b4ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375843"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="f31a6-103">Código-fonte WebHooks do ASP.NET e pacotes do NuGet</span><span class="sxs-lookup"><span data-stu-id="f31a6-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="f31a6-104">WebHooks do Microsoft ASP.NET faz parte da família de módulos do Microsoft ASP.NET e está hospedado como um [Abrir projeto de código-fonte no GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="f31a6-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="f31a6-105">Isso significa que podemos aceitar contribuições, mas examine os [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação de pull.</span><span class="sxs-lookup"><span data-stu-id="f31a6-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="f31a6-106">Esta documentação online que você está lendo agora também está hospedada como [software livre no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.</span><span class="sxs-lookup"><span data-stu-id="f31a6-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="f31a6-107">Pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="f31a6-107">NuGet packages</span></span>

<span data-ttu-id="f31a6-108">O [pacotes do NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são divididas em três partes:</span><span class="sxs-lookup"><span data-stu-id="f31a6-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="f31a6-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): um pacote comum que é compartilhado entre remetentes e receptores.</span><span class="sxs-lookup"><span data-stu-id="f31a6-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="f31a6-110">[Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): um conjunto de pacotes que dão suporte a enviar seus próprios WebHooks a outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="f31a6-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="f31a6-111">A funcionalidade para o envio de WebHooks é descrita mais detalhadamente na [WebHooks enviando](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="f31a6-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="f31a6-112">[Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): um conjunto de pacotes que dão suporte a recebimento de WebHooks de outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="f31a6-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="f31a6-113">A funcionalidade para o recebimento de WebHooks é descrita mais detalhadamente na [WebHooks recebendo](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="f31a6-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
