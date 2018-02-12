---
title: Impondo HTTPS em um aplicativo do ASP.NET Core
author: rick-anderson
description: "Mostra como exigir HTTPS/TLS em um núcleo de ASP.NET aplicativo web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Impondo HTTPS em um aplicativo do ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento mostra como:

- Exigir HTTPS para todas as solicitações.
- Redirecione todas as solicitações HTTP para HTTPS.

> [!WARNING]
> Fazer **não** usar `RequireHttpsAttribute` em APIs da Web que recebe informações confidenciais. `RequireHttpsAttribute`usa códigos de status HTTP para redirecionar navegadores de HTTP para HTTPS. Clientes de API podem não entender ou obedecer redirecionamentos de HTTP para HTTPS. Esses clientes podem enviar informações sobre HTTP. APIs da Web deverá:
>
>* Não escuta no HTTP.
>* Feche a conexão com o código de status 400 (solicitação incorreta) e não atender à solicitação.

## <a name="require-https"></a>Exigir HTTPS

O [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) é usada para exigir HTTPS. `[RequireHttpsAttribute]`pode decorar controladores ou métodos, ou podem ser aplicadas globalmente. Para aplicar o atributo global, adicione o seguinte código ao `ConfigureServices` em `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

O código realçado anterior requer todas as solicitações usar `HTTPS`; portanto, as solicitações HTTP são ignoradas. O seguinte código realçado redireciona todas as solicitações HTTP para HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obter mais informações, consulte [Middleware de regravação de URL](xref:fundamentals/url-rewriting).

Exigir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) é uma prática recomendada de segurança. Aplicar o `[RequireHttps]` atributo para todas as páginas Razor/controladores não é considerado mais seguro que exijam HTTPS globalmente. Você não pode garantir a `[RequireHttps]` atributo é aplicado quando forem adicionadas novos controladores e páginas Razor.