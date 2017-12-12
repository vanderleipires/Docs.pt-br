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
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="4d4ef-103">ASP.NET WebHooks log</span><span class="sxs-lookup"><span data-stu-id="4d4ef-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="4d4ef-104">Microsoft ASP.NET WebHooks usa o registro em log como uma forma de emissão de relatórios de problemas.</span><span class="sxs-lookup"><span data-stu-id="4d4ef-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="4d4ef-105">Por padrão os logs são gravados usando [Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) onde pode ser gerenciadas usando [ouvintes de rastreamento](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) como qualquer outro fluxo de log.</span><span class="sxs-lookup"><span data-stu-id="4d4ef-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="4d4ef-106">Ao implantar seu aplicativo da Web como um aplicativo Web do Azure, os logs são selecionados automaticamente e podem ser gerenciados junto com qualquer outro [Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) log.</span><span class="sxs-lookup"><span data-stu-id="4d4ef-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="4d4ef-107">Para obter detalhes, consulte [habilitar o log de diagnóstico para aplicativos web no serviço de aplicativo do Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="4d4ef-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="4d4ef-108">Além disso, os logs podem ser obtidos diretamente do Visual Studio, conforme descrito em [solucionar problemas de um aplicativo web no serviço de aplicativo do Azure usando o Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="4d4ef-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="4d4ef-109">Logs de redirecionamento</span><span class="sxs-lookup"><span data-stu-id="4d4ef-109">Redirecting Logs</span></span>

<span data-ttu-id="4d4ef-110">Em vez de gravar logs de [Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), é possível fornecer uma implementação de log alternativo que pode fazer logon diretamente a um Gerenciador de log como [Log4Net](http://logging.apache.org/log4net/) e [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="4d4ef-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="4d4ef-111">Basta fornecer uma implementação de [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) e registrá-lo com um mecanismo de injeção de dependência de sua escolha e ele será escolhido pelo Microsoft ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="4d4ef-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="4d4ef-112">Consulte [injeção de dependência no ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="4d4ef-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
