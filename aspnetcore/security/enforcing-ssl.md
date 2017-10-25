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
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Garantindo SSL em uma aplicação ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento demonstra como:

- Exigir SSL em todas as requisições (através de HTTPS).
- Redirecionar todas as requisições HTTP para HTTPS.

## <a name="require-ssl"></a>Garantindo SSL

O [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) é utilizado para garantir a conexão por SSL. Além de você poder decorar métodos ou controladores com esse atributo, ele também pode ser aplicado globalmente, conforme é demonstrado abaixo:

Adicione o seguinte código em `ConfigureServices` dentro de `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

O código acima requere que todas as solicitações utilizem `HTTPS`. Ou seja, solicitações HTTP serão ignoradas. O código abaixo redireciona todas as solicitações HTTP para HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting) para obter mais informações.

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Note que simplesmente aplicar o atributo `[RequireHttps]` em todo controlador não é mais considerada uma prática tão segura quanto exigir HTTPS globalmente, uma vez que você não pode garantir que novos controladores adicionados à sua aplicação utilizarão o atributo `[RequireHttps]`.
