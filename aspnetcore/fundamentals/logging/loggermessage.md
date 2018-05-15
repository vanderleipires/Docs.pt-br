---
title: Registro em log de alto desempenho com o LoggerMessage no ASP.NET Core
author: guardrex
description: Saiba como usar o LoggerMessage para criar representantes que podem ser armazenados em cache e exigem menos alocações de objeto para cenários de registro em log de alto desempenho.
manager: wpickett
ms.author: riande
ms.date: 11/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: 24a75cfacfa61ca66e78deeb743baa75718dfb76
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="8fe27-103">Registro em log de alto desempenho com o LoggerMessage no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8fe27-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="8fe27-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8fe27-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8fe27-105">Os recursos do [LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) criam delegados armazenáveis em cache que exigem menos alocações de objeto e sobrecarga de computação reduzida comparado aos [métodos de extensão do agente](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug` e `LogError`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="8fe27-106">Para cenários de registro em log de alto desempenho, use o padrão `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="8fe27-107">`LoggerMessage` fornece as seguintes vantagens de desempenho em relação aos métodos de extensão do Agente:</span><span class="sxs-lookup"><span data-stu-id="8fe27-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="8fe27-108">Métodos de extensão do agente exigem tipos de valor de conversão boxing, como `int`, em `object`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="8fe27-109">O padrão `LoggerMessage` evita a conversão boxing usando campos `Action` estáticos e métodos de extensão com parâmetros fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="8fe27-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="8fe27-110">Os métodos de extensão do agente precisam analisar o modelo de mensagem (cadeia de caracteres de formato nomeada) sempre que uma mensagem de log é gravada.</span><span class="sxs-lookup"><span data-stu-id="8fe27-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="8fe27-111">`LoggerMessage` exige apenas a análise de um modelo uma vez quando a mensagem é definida.</span><span class="sxs-lookup"><span data-stu-id="8fe27-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="8fe27-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8fe27-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8fe27-113">O aplicativo de exemplo demonstra recursos do `LoggerMessage` com um sistema básico de acompanhamento de aspas.</span><span class="sxs-lookup"><span data-stu-id="8fe27-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="8fe27-114">O aplicativo adiciona e exclui aspas usando um banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="8fe27-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="8fe27-115">Conforme ocorrem essas operações, são geradas mensagens de log usando o padrão `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="8fe27-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="8fe27-116">LoggerMessage.Define</span></span>

<span data-ttu-id="8fe27-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) cria um delegado `Action` para registrar uma mensagem em log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="8fe27-118">Sobrecargas de `Define` permitem passar até seis parâmetros de tipo para uma cadeia de caracteres de formato nomeada (modelo).</span><span class="sxs-lookup"><span data-stu-id="8fe27-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="8fe27-119">A cadeia de caracteres fornecida para o método `Define` é um modelo e não uma cadeia de caracteres interpolada.</span><span class="sxs-lookup"><span data-stu-id="8fe27-119">The string provided to the `Define` method is a template and not an interpolated string.</span></span> <span data-ttu-id="8fe27-120">Os espaços reservados são preenchidos na ordem em que os tipos são especificados.</span><span class="sxs-lookup"><span data-stu-id="8fe27-120">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="8fe27-121">Os nomes do espaço reservado no modelo devem ser descritivos e consistentes em todos os modelos.</span><span class="sxs-lookup"><span data-stu-id="8fe27-121">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="8fe27-122">Eles servem como nomes de propriedade em dados de log estruturado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-122">They serve as property names within structured log data.</span></span> <span data-ttu-id="8fe27-123">Recomendamos o uso da [formatação Pascal Case](/dotnet/standard/design-guidelines/capitalization-conventions) para nomes de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-123">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="8fe27-124">Por exemplo, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-124">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="8fe27-125">Cada mensagem de log é uma `Action` mantida em um campo estático criado por `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-125">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="8fe27-126">Por exemplo, o aplicativo de exemplo cria um campo para descrever uma mensagem de log para uma solicitação GET para a página de Índice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8fe27-126">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="8fe27-127">Para a `Action`, especifique:</span><span class="sxs-lookup"><span data-stu-id="8fe27-127">For the `Action`, specify:</span></span>

* <span data-ttu-id="8fe27-128">O nível de log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-128">The log level.</span></span>
* <span data-ttu-id="8fe27-129">Um identificador de evento exclusivo ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) com o nome do método de extensão estático.</span><span class="sxs-lookup"><span data-stu-id="8fe27-129">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="8fe27-130">O modelo de mensagem (cadeia de caracteres de formato nomeada).</span><span class="sxs-lookup"><span data-stu-id="8fe27-130">The message template (named format string).</span></span> 

<span data-ttu-id="8fe27-131">Uma solicitação para a página de Índice do aplicativo de exemplo define:</span><span class="sxs-lookup"><span data-stu-id="8fe27-131">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="8fe27-132">O nível de log como `Information`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-132">Log level to `Information`.</span></span>
* <span data-ttu-id="8fe27-133">A ID do evento como `1` com o nome do método `IndexPageRequested`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-133">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="8fe27-134">Modelo de mensagem (cadeia de caracteres de formato nomeada) como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="8fe27-134">Message template (named format string) to a string.</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="8fe27-135">Repositórios de log estruturado podem usar o nome do evento quando recebem a ID do evento para enriquecer o log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-135">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="8fe27-136">Por exemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa o nome do evento.</span><span class="sxs-lookup"><span data-stu-id="8fe27-136">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="8fe27-137">A `Action` é invocada por meio de um método de extensão fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-137">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="8fe27-138">O método `IndexPageRequested` registra uma mensagem para uma solicitação GET da página de Índice no aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="8fe27-138">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="8fe27-139">`IndexPageRequested` é chamado no agente no método `OnGetAsync` em *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8fe27-139">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="8fe27-140">Inspecione a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="8fe27-140">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="8fe27-141">Para passar parâmetros para uma mensagem de log, defina até seis tipos ao criar o campo estático.</span><span class="sxs-lookup"><span data-stu-id="8fe27-141">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="8fe27-142">O aplicativo de exemplo registra uma cadeia de caracteres em log ao adicionar aspas definindo um tipo `string` para o campo `Action`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-142">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="8fe27-143">O modelo de mensagem de log do delegado recebe seus valores de espaço reservado dos tipos fornecidos.</span><span class="sxs-lookup"><span data-stu-id="8fe27-143">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="8fe27-144">O aplicativo de exemplo define um delegado para adicionar aspas quando o parâmetro de aspas é uma `string`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-144">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="8fe27-145">O método de extensão estático para adicionar aspas, `QuoteAdded`, recebe o valor do argumento de aspas e passa-o para o delegado `Action`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-145">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="8fe27-146">No modelo da página de Índice (*Pages/Index.cshtml.cs*), `QuoteAdded` é chamado para registrar a mensagem em log:</span><span class="sxs-lookup"><span data-stu-id="8fe27-146">In the Index page's page model (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="8fe27-147">Inspecione a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="8fe27-147">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="8fe27-148">O aplicativo de exemplo implementa um padrão `try`&ndash;`catch` para a exclusão de aspas.</span><span class="sxs-lookup"><span data-stu-id="8fe27-148">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="8fe27-149">Uma mensagem informativa é registrada em log para uma operação de exclusão bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="8fe27-149">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="8fe27-150">Uma mensagem de erro é registrada em log para uma operação de exclusão quando uma exceção é gerada.</span><span class="sxs-lookup"><span data-stu-id="8fe27-150">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="8fe27-151">A mensagem de log para a operação de exclusão sem êxito inclui o rastreamento de pilha da exceção (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8fe27-151">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="8fe27-152">Observe como a exceção é passada para o delegado em `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-152">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="8fe27-153">No modelo da página de Índice, uma exclusão de aspas bem-sucedida chama o método `QuoteDeleted` no agente.</span><span class="sxs-lookup"><span data-stu-id="8fe27-153">In the page model for the Index page, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="8fe27-154">Quando as aspas não são encontradas para exclusão, uma `ArgumentNullException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="8fe27-154">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="8fe27-155">A exceção é interceptada pela instrução `try`&ndash;`catch` e registrada em log com uma chamada ao método `QuoteDeleteFailed` no agente no bloco `catch` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="8fe27-155">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="8fe27-156">Quando as aspas forem excluídas com êxito, inspecione a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="8fe27-156">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="8fe27-157">Quando a exclusão de aspas falha, inspecione a saída do console do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8fe27-157">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="8fe27-158">Observe que a exceção é incluída na mensagem de log:</span><span class="sxs-lookup"><span data-stu-id="8fe27-158">Note that the exception is included in the log message:</span></span>

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a><span data-ttu-id="8fe27-159">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="8fe27-159">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="8fe27-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) cria um delegado `Func` para definir um [escopo de log](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="8fe27-160">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="8fe27-161">Sobrecargas de `DefineScope` permitem passar até três parâmetros de tipo para uma cadeia de caracteres de formato nomeada (modelo).</span><span class="sxs-lookup"><span data-stu-id="8fe27-161">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="8fe27-162">Como é o caso com o método `Define`, a cadeia de caracteres fornecida ao método `DefineScope` é um modelo e não uma cadeia de caracteres interpolada.</span><span class="sxs-lookup"><span data-stu-id="8fe27-162">As is the case with the `Define` method, the string provided to the `DefineScope` method is a template and not an interpolated string.</span></span> <span data-ttu-id="8fe27-163">Os espaços reservados são preenchidos na ordem em que os tipos são especificados.</span><span class="sxs-lookup"><span data-stu-id="8fe27-163">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="8fe27-164">Os nomes do espaço reservado no modelo devem ser descritivos e consistentes em todos os modelos.</span><span class="sxs-lookup"><span data-stu-id="8fe27-164">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="8fe27-165">Eles servem como nomes de propriedade em dados de log estruturado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-165">They serve as property names within structured log data.</span></span> <span data-ttu-id="8fe27-166">Recomendamos o uso da [formatação Pascal Case](/dotnet/standard/design-guidelines/capitalization-conventions) para nomes de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-166">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="8fe27-167">Por exemplo, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-167">For example, `{Count}`, `{FirstName}`.</span></span>

<span data-ttu-id="8fe27-168">Defina um [escopo de log](xref:fundamentals/logging/index#log-scopes) a ser aplicado a uma série de mensagens de log usando o método [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope).</span><span class="sxs-lookup"><span data-stu-id="8fe27-168">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="8fe27-169">O aplicativo de exemplo tem um botão **Limpar Tudo** para excluir todas as aspas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8fe27-169">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="8fe27-170">As aspas são excluídas com a remoção das aspas individualmente, uma por vez.</span><span class="sxs-lookup"><span data-stu-id="8fe27-170">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="8fe27-171">Sempre que aspas são excluídas, o método `QuoteDeleted` é chamado no agente.</span><span class="sxs-lookup"><span data-stu-id="8fe27-171">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="8fe27-172">Um escopo de log é adicionado a essas mensagens de log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-172">A log scope is added to these log messages.</span></span>

<span data-ttu-id="8fe27-173">Habilite `IncludeScopes` nas opções do agente do console:</span><span class="sxs-lookup"><span data-stu-id="8fe27-173">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[](loggermessage/sample/Program.cs?name=snippet1&highlight=10)]

<span data-ttu-id="8fe27-174">A configuração de `IncludeScopes` é necessária em aplicativos ASP.NET Core 2.0 para habilitar os escopos de log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-174">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="8fe27-175">A configuração de `IncludeScopes` por meio dos arquivos de configuração *appsettings* é um recurso que foi planejado para a versão ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8fe27-175">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="8fe27-176">O aplicativo de exemplo limpa outros provedores e adiciona filtros para reduzir a saída de log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-176">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="8fe27-177">Isso facilita a visualização das mensagens de log da amostra que demonstram os recursos de `LoggerMessage`.</span><span class="sxs-lookup"><span data-stu-id="8fe27-177">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="8fe27-178">Para criar um escopo de log, adicione um campo para conter um delegado `Func` para o escopo.</span><span class="sxs-lookup"><span data-stu-id="8fe27-178">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="8fe27-179">O aplicativo de exemplo cria um campo chamado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="8fe27-179">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="8fe27-180">Use `DefineScope` para criar o delegado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-180">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="8fe27-181">Até três tipos podem ser especificados para uso como argumentos de modelo quando o delegado é invocado.</span><span class="sxs-lookup"><span data-stu-id="8fe27-181">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="8fe27-182">O aplicativo de exemplo usa um modelo de mensagem que inclui o número de aspas excluídas (um tipo `int`):</span><span class="sxs-lookup"><span data-stu-id="8fe27-182">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="8fe27-183">Forneça um método de extensão estático para a mensagem de log.</span><span class="sxs-lookup"><span data-stu-id="8fe27-183">Provide a static extension method for the log message.</span></span> <span data-ttu-id="8fe27-184">Inclua os parâmetros de tipo para propriedades nomeadas exibidos no modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="8fe27-184">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="8fe27-185">O aplicativo de exemplo usa uma `count` de aspas a ser excluída e retorna `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-185">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="8fe27-186">O escopo encapsula as chamadas de extensão de log em um bloco `using`:</span><span class="sxs-lookup"><span data-stu-id="8fe27-186">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="8fe27-187">Inspecione as mensagens de log na saída do console do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8fe27-187">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="8fe27-188">O seguinte resultado mostra três aspas excluídas com a mensagem de escopo de log incluída:</span><span class="sxs-lookup"><span data-stu-id="8fe27-188">The following result shows three quotes deleted with the log scope message included:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="see-also"></a><span data-ttu-id="8fe27-189">Consulte também</span><span class="sxs-lookup"><span data-stu-id="8fe27-189">See also</span></span>

* [<span data-ttu-id="8fe27-190">Registro em log</span><span class="sxs-lookup"><span data-stu-id="8fe27-190">Logging</span></span>](xref:fundamentals/logging/index)
