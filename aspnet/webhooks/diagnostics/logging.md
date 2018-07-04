---
uid: webhooks/diagnostics/logging
title: WebHooks de registro em log do ASP.NET | Microsoft Docs
author: rick-anderson
description: Como fazer o registro em log no ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.openlocfilehash: 5501d250ea6dd86c0cfddec8d9941ec16b4f57ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364100"
---
# <a name="aspnet-webhooks-logging"></a>WebHooks de registro em log do ASP.NET

WebHooks do Microsoft ASP.NET usa o registro em log como uma maneira de problemas e problemas de relatório. Por padrão os logs são gravados usando [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) onde eles podem ser gerenciados usando [ouvintes de rastreamento](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) como qualquer outro fluxo de log.

Ao implantar seu aplicativo Web como um aplicativo Web do Azure, os logs serão selecionados automaticamente e podem ser gerenciados junto com qualquer outro [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) registro em log. Para obter detalhes, consulte [habilitar o log de diagnóstico para aplicativos web no serviço de aplicativo do Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Além disso, os logs podem ser obtidos diretamente de dentro do Visual Studio, conforme descrito em [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirecionar Logs

Em vez de gravar logs [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), é possível fornecer uma implementação de registro em log alternativo que pode fazer logon diretamente para um Gerenciador de log, como [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/). Simplesmente fornecer uma implementação [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrá-lo com um mecanismo de injeção de dependência de sua preferência e ele será recebido pelo Microsoft ASP.NET WebHooks. Consulte [injeção de dependência no ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obter detalhes.
