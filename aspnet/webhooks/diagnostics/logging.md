---
uid: webhooks/diagnostics/logging
title: ASP.NET WebHooks log | Microsoft Docs
author: rick-anderson
description: Como fazer logon WebHooks ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044819"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET WebHooks log

Microsoft ASP.NET WebHooks usa o registro em log como uma forma de emissão de relatórios de problemas. Por padrão os logs são gravados usando [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) onde pode ser gerenciadas usando [ouvintes de rastreamento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como qualquer outro fluxo de log.

Ao implantar seu aplicativo da Web como um aplicativo Web do Azure, os logs são selecionados automaticamente e podem ser gerenciados junto com qualquer outro [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) log. Para obter detalhes, consulte [habilitar o log de diagnóstico para aplicativos web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Além disso, os logs podem ser obtidos diretamente do Visual Studio, conforme descrito em [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Logs de redirecionamento

Em vez de gravar logs de [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), é possível fornecer uma implementação de log alternativo que pode fazer logon diretamente a um Gerenciador de log como [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/). Basta fornecer uma implementação de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrá-lo com um mecanismo de injeção de dependência de sua escolha e ele será escolhido pelo Microsoft ASP.NET WebHooks. Consulte [injeção de dependência no ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obter detalhes.
