---
title: "Imposição de SSL em um aplicativo do ASP.NET Core"
author: rick-anderson
description: "Mostra como exigir SSL em um núcleo de ASP.NET aplicativo web"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Imposição de SSL em um aplicativo do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento mostra como:

- Exigir SSL para todas as solicitações (somente para solicitações HTTPS).
- Redirecione todas as solicitações HTTP para HTTPS.

## <a name="require-ssl"></a>Exigir SSL

O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é usada para exigir SSL. Você pode decorar métodos com esse atributo ou controladores ou aplicá-lo globalmente, conforme mostrado abaixo:

Adicione o seguinte código para `ConfigureServices` em `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

O código acima requer que todas as solicitações usar `HTTPS`, portanto, as solicitações HTTP são ignoradas. O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Aplicar o `[RequireHttps]` atributo para todos os do controlador não é considerado mais seguro que exijam HTTPS globalmente. Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.
