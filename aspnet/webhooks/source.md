---
uid: webhooks/source
title: Código-fonte WebHooks do ASP.NET e pacotes do NuGet | Microsoft Docs
author: rick-anderson
description: Links para código-fonte WebHooks do ASP.NET e pacotes do NuGet
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 1ee4adc2a28054ed1f856d7e4e991b34972a70e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802161"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código-fonte WebHooks do ASP.NET e pacotes do NuGet

WebHooks do Microsoft ASP.NET faz parte da família de módulos do Microsoft ASP.NET e está hospedado como um [Abrir projeto de código-fonte no GitHub](https://github.com/aspnet/WebHooks). Isso significa que podemos aceitar contribuições, mas examine os [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação de pull.

Esta documentação online que você está lendo agora também está hospedada como [software livre no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.

## <a name="nuget-packages"></a>Pacotes NuGet

O [pacotes do NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são divididas em três partes:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): um pacote comum que é compartilhado entre remetentes e receptores.

* [Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): um conjunto de pacotes que dão suporte a enviar seus próprios WebHooks a outras pessoas. A funcionalidade para o envio de WebHooks é descrita mais detalhadamente na [WebHooks enviando](sending/index.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): um conjunto de pacotes que dão suporte a recebimento de WebHooks de outras pessoas. A funcionalidade para o recebimento de WebHooks é descrita mais detalhadamente na [WebHooks recebendo](receiving/index.md).
