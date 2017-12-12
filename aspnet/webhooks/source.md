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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET WebHooks código-fonte e pacotes do NuGet

Microsoft ASP.NET WebHooks faz parte da família Microsoft ASP.NET de módulos e está hospedado como um [Abrir projeto de origem no GitHub](https://github.com/aspnet/WebHooks). Isso significa que podemos aceitar contribuições, mas examine o [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação pull.

Esta documentação online que você está lendo agora também está hospedada como [código-fonte aberto no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.

## <a name="nuget-packages"></a>Pacotes do NuGet

Microsoft ASP.NET WebHooks também está disponível como visualização pacotes Nuget, que significa que você precisa selecionar o sinalizador de visualização no Visual Studio para vê-los.

O [pacotes do Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são devided em três partes:

* [Comuns](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): um pacote comum que é compartilhado entre remetentes e destinatários.

* [Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): um conjunto de pacotes com suporte enviar seus próprios WebHooks a outras pessoas. A funcionalidade para enviar WebHooks é descrita mais detalhadamente na [WebHooks enviar](sending/index.md).

* [Receptores de](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): um conjunto de pacotes com suporte receber WebHooks de outros. A funcionalidade para receber WebHooks é descrita mais detalhadamente na [WebHooks recebendo](receiving/index.md).
