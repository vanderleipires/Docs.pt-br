---
title: "Imposição de SSL em um aplicativo do ASP.NET Core"
author: rick-anderson
description: "Mostra como exigir SSL em um núcleo de ASP.NET aplicativo web"
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2017
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

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Aplicar o `[RequireHttps]` atributo ao controlador todos os não é considerado mais seguro que exijam HTTPS globalmente. Você não pode garantir lembrará novos controladores adicionados ao seu aplicativo aplicar o `[RequireHttps]` atributo.

## <a name="set-up-iis-express-for-sslhttps"></a>Configurar o IIS Express para HTTPS/SSL

Consulte [configurar o HTTPS para o desenvolvimento em ASP.NET Core](xref:security/https#iisxpress).
