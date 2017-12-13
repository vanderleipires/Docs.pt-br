---
title: "Registro em log de alto desempenho com LoggerMessage no núcleo do ASP.NET"
author: guardrex
description: "Saiba como usar recursos de LoggerMessage para criar representantes armazenável em cache que exigem menos alocações de objeto que os métodos de extensão do agente de log para cenários de registro em log de alto desempenho."
ms.author: riande
manager: wpickett
ms.date: 11/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: defba75c6c9ea13d24af4cd8515d82d9e7cf9853
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a><span data-ttu-id="1d0fd-103">Registro em log de alto desempenho com LoggerMessage no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1d0fd-103">High-performance logging with LoggerMessage in ASP.NET Core</span></span>

<span data-ttu-id="1d0fd-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1d0fd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1d0fd-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) recursos criam armazenável delegados que exigem menos alocações de objeto e reduziu a sobrecarga de computação que [métodos de extensão do agente de log](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug`e `LogError`.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-105">[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) features create cacheable delegates that require fewer object allocations and reduced computational overhead than [logger extension methods](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), such as `LogInformation`, `LogDebug`, and `LogError`.</span></span> <span data-ttu-id="1d0fd-106">Para cenários de registro em log de alto desempenho, use o `LoggerMessage` padrão.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-106">For high-performance logging scenarios, use the `LoggerMessage` pattern.</span></span>

<span data-ttu-id="1d0fd-107">`LoggerMessage`Fornece as seguintes vantagens de desempenho sobre métodos de extensão do agente de log:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-107">`LoggerMessage` provides the following performance advantages over Logger extension methods:</span></span>

* <span data-ttu-id="1d0fd-108">Métodos de extensão do agente de log exigem tipos de valor de "boxing" (converter), como `int`, em `object`.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-108">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="1d0fd-109">O `LoggerMessage` padrão evita conversão boxing usando estático `Action` campos e métodos de extensão com parâmetros fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-109">The `LoggerMessage` pattern avoids boxing by using static `Action` fields and extension methods with strongly-typed parameters.</span></span>
* <span data-ttu-id="1d0fd-110">Métodos de extensão do agente devem analisar o modelo de mensagem (cadeia de caracteres de formato nomeado) toda vez que uma mensagem de log é gravada.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-110">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="1d0fd-111">`LoggerMessage`exige apenas analisar um modelo uma vez quando a mensagem é definida.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-111">`LoggerMessage` only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="1d0fd-112">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d0fd-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1d0fd-113">O aplicativo de exemplo demonstra `LoggerMessage` recursos com aspas básica sistema de controle.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-113">The sample app demonstrates `LoggerMessage` features with a basic quote tracking system.</span></span> <span data-ttu-id="1d0fd-114">O aplicativo adiciona e exclui aspas usando um banco de dados na memória.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-114">The app adds and deletes quotes using an in-memory database.</span></span> <span data-ttu-id="1d0fd-115">Como essas operações, mensagens de log são geradas usando o `LoggerMessage` padrão.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-115">As these operations occur, log messages are generated using the `LoggerMessage` pattern.</span></span>

## <a name="loggermessagedefine"></a><span data-ttu-id="1d0fd-116">LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="1d0fd-116">LoggerMessage.Define</span></span>

<span data-ttu-id="1d0fd-117">[Definir (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) cria um `Action` delegar para registrar uma mensagem.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-117">[Define(LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) creates an `Action` delegate for logging a message.</span></span> <span data-ttu-id="1d0fd-118">`Define`sobrecargas permitem passar até seis parâmetros de tipo para uma cadeia de caracteres de formato nomeado (modelo).</span><span class="sxs-lookup"><span data-stu-id="1d0fd-118">`Define` overloads permit passing up to six type parameters to a named format string (template).</span></span>

## <a name="loggermessagedefinescope"></a><span data-ttu-id="1d0fd-119">LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="1d0fd-119">LoggerMessage.DefineScope</span></span>

<span data-ttu-id="1d0fd-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) cria um `Func` delegar para definir um [escopo de log](xref:fundamentals/logging/index#log-scopes).</span><span class="sxs-lookup"><span data-stu-id="1d0fd-120">[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) creates a `Func` delegate for defining a [log scope](xref:fundamentals/logging/index#log-scopes).</span></span> <span data-ttu-id="1d0fd-121">`DefineScope`sobrecargas permitem passar até três parâmetros de tipo para uma cadeia de caracteres de formato nomeado (modelo).</span><span class="sxs-lookup"><span data-stu-id="1d0fd-121">`DefineScope` overloads permit passing up to three type parameters to a named format string (template).</span></span>

## <a name="message-template-named-format-string"></a><span data-ttu-id="1d0fd-122">Modelo de mensagem (chamado de cadeia de caracteres de formato)</span><span class="sxs-lookup"><span data-stu-id="1d0fd-122">Message template (named format string)</span></span>

<span data-ttu-id="1d0fd-123">A cadeia de caracteres fornecida para o `Define` e `DefineScope` métodos é um modelo e não uma cadeia de caracteres interpolada.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-123">The string provided to the `Define` and `DefineScope` methods is a template and not an interpolated string.</span></span> <span data-ttu-id="1d0fd-124">Espaços reservados são preenchidos na ordem em que os tipos são especificados.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-124">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="1d0fd-125">Nomes de espaço reservado no modelo devem ser descritivo e consistente em modelos.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-125">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="1d0fd-126">Eles servem como nomes de propriedade em dados estruturados de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-126">They serve as property names within structured log data.</span></span> <span data-ttu-id="1d0fd-127">É recomendável [Pascal maiusculas e minúsculas](/dotnet/standard/design-guidelines/capitalization-conventions) para nomes de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-127">We recommend [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) for placeholder names.</span></span> <span data-ttu-id="1d0fd-128">Por exemplo, `{Count}`, `{FirstName}`.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-128">For example, `{Count}`, `{FirstName}`.</span></span>

## <a name="implementing-loggermessagedefine"></a><span data-ttu-id="1d0fd-129">Implementando LoggerMessage.Define</span><span class="sxs-lookup"><span data-stu-id="1d0fd-129">Implementing LoggerMessage.Define</span></span>

<span data-ttu-id="1d0fd-130">Cada mensagem de log é um `Action` mantido em um campo estático criado por `LoggerMessage.Define`.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-130">Each log message is an `Action` held in a static field created by `LoggerMessage.Define`.</span></span> <span data-ttu-id="1d0fd-131">Por exemplo, o aplicativo de exemplo cria um campo para descrever uma mensagem de log para uma solicitação GET para a página de índice (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d0fd-131">For example, the sample app creates a field to describe a log message for a GET request for the Index page (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

<span data-ttu-id="1d0fd-132">Para o `Action`, especifique:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-132">For the `Action`, specify:</span></span>

* <span data-ttu-id="1d0fd-133">O nível de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-133">The log level.</span></span>
* <span data-ttu-id="1d0fd-134">Um identificador exclusivo do evento ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) com o nome do método de extensão estático.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-134">A unique event identifier ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) with the name of the static extension method.</span></span>
* <span data-ttu-id="1d0fd-135">O modelo de mensagem (chamado de cadeia de caracteres de formato).</span><span class="sxs-lookup"><span data-stu-id="1d0fd-135">The message template (named format string).</span></span> 

<span data-ttu-id="1d0fd-136">Uma solicitação para a página de índice dos conjuntos de aplicativo de exemplo a:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-136">A request for the Index page of the sample app sets the:</span></span>

* <span data-ttu-id="1d0fd-137">Nível de log `Information`.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-137">Log level to `Information`.</span></span>
* <span data-ttu-id="1d0fd-138">Id do evento para `1` com o nome do `IndexPageRequested` método.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-138">Event id to `1` with the name of the `IndexPageRequested` method.</span></span>
* <span data-ttu-id="1d0fd-139">Modelo de mensagem (chamado de cadeia de caracteres de formato) para uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-139">Message template (named format string) to a string.</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

<span data-ttu-id="1d0fd-140">Repositórios de log estruturado podem usar o nome do evento quando ele é fornecido com a id de evento para enriquecer o registro em log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-140">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="1d0fd-141">Por exemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa o nome do evento.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-141">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="1d0fd-142">O `Action` é invocado por meio de um método de extensão fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-142">The `Action` is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="1d0fd-143">O `IndexPageRequested` método registra uma mensagem para uma solicitação de obtenção de página de índice no aplicativo de exemplo:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-143">The `IndexPageRequested` method logs a message for an Index page GET request in the sample app:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

<span data-ttu-id="1d0fd-144">`IndexPageRequested`é chamado no agente de log no `OnGetAsync` método *Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-144">`IndexPageRequested` is called on the logger in the `OnGetAsync` method in *Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

<span data-ttu-id="1d0fd-145">Inspecione a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-145">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

<span data-ttu-id="1d0fd-146">Para passar parâmetros para uma mensagem de log, defina até seis tipos ao criar o campo estático.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-146">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="1d0fd-147">O aplicativo de exemplo registra uma cadeia de caracteres ao adicionar uma cotação definindo um `string` de tipo para o `Action` campo:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-147">The sample app logs a string when adding a quote by defining a `string` type for the `Action` field:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

<span data-ttu-id="1d0fd-148">Modelo de mensagem de log do representante recebe seus valores de espaço reservado de tipos fornecidos.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-148">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="1d0fd-149">O aplicativo de exemplo define um delegado para adicionar uma cotação onde o parâmetro de cotação é um `string`:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-149">The sample app defines a delegate for adding a quote where the quote parameter is a `string`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

<span data-ttu-id="1d0fd-150">O método de extensão estático para adicionar uma cotação, `QuoteAdded`, recebe o valor do argumento aspas e passa para o `Action` delegar:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-150">The static extension method for adding a quote, `QuoteAdded`, receives the quote argument value and passes it to the `Action` delegate:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

<span data-ttu-id="1d0fd-151">No arquivo de code-behind da página de índice (*Pages/Index.cshtml.cs*), `QuoteAdded` é chamado para registrar a mensagem:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-151">In the Index page's code-behind file (*Pages/Index.cshtml.cs*), `QuoteAdded` is called to log the message:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

<span data-ttu-id="1d0fd-152">Inspecione a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-152">Inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

<span data-ttu-id="1d0fd-153">A exemplo aplicativo implementa uma `try` &ndash; `catch` padrão para exclusão de aspas.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-153">The sample app implements a `try`&ndash;`catch` pattern for quote deletion.</span></span> <span data-ttu-id="1d0fd-154">Uma mensagem informativa é registrada para uma operação de exclusão com êxito.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-154">An informational message is logged for a successful delete operation.</span></span> <span data-ttu-id="1d0fd-155">Uma mensagem de erro é registrada para uma operação de exclusão quando uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-155">An error message is logged for a delete operation when an exception is thrown.</span></span> <span data-ttu-id="1d0fd-156">A mensagem de log para a operação de exclusão de êxito inclui o rastreamento de pilha de exceção (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d0fd-156">The log message for the unsuccessful delete operation includes the exception stack trace (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

<span data-ttu-id="1d0fd-157">Observe como a exceção é passada para o representante em `QuoteDeleteFailed`:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-157">Note how the exception is passed to the delegate in `QuoteDeleteFailed`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

<span data-ttu-id="1d0fd-158">No índice página code-behind, a exclusão bem-sucedida de cotação chama o `QuoteDeleted` método no agente de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-158">In the Index page code-behind, a successful quote deletion calls the `QuoteDeleted` method on the logger.</span></span> <span data-ttu-id="1d0fd-159">Quando uma cotação não foi encontrada para exclusão, um `ArgumentNullException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-159">When a quote isn't found for deletion, an `ArgumentNullException` is thrown.</span></span> <span data-ttu-id="1d0fd-160">A exceção é interceptada pelo `try` &ndash; `catch` instrução e conectado ao chamar o `QuoteDeleteFailed` método no agente de log no `catch` bloco (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d0fd-160">The exception is trapped by the `try`&ndash;`catch` statement and logged by calling the `QuoteDeleteFailed` method on the logger in the `catch` block (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

<span data-ttu-id="1d0fd-161">Quando uma cotação é excluída com êxito, verifique a saída do console do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-161">When a quote is successfully deleted, inspect the app's console output:</span></span>

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

<span data-ttu-id="1d0fd-162">Quando ocorre falha na exclusão de cotação, inspecione a saída do console do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-162">When quote deletion fails, inspect the app's console output.</span></span> <span data-ttu-id="1d0fd-163">Observe que a exceção é incluída na mensagem de log:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-163">Note that the exception is included in the log message:</span></span>

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

## <a name="implementing-loggermessagedefinescope"></a><span data-ttu-id="1d0fd-164">Implementando LoggerMessage.DefineScope</span><span class="sxs-lookup"><span data-stu-id="1d0fd-164">Implementing LoggerMessage.DefineScope</span></span>

<span data-ttu-id="1d0fd-165">Definir um [escopo de log](xref:fundamentals/logging/index#log-scopes) para aplicar a uma série de mensagens de log usando o [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) método.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-165">Define a [log scope](xref:fundamentals/logging/index#log-scopes) to apply to a series of log messages using the [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) method.</span></span>

<span data-ttu-id="1d0fd-166">O aplicativo de exemplo tem uma **Limpar tudo** botão para excluir todas as aspas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-166">The sample app has a **Clear All** button for deleting all of the quotes in the database.</span></span> <span data-ttu-id="1d0fd-167">As aspas são excluídas, removendo-os um por vez.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-167">The quotes are deleted by removing them one at a time.</span></span> <span data-ttu-id="1d0fd-168">Cada vez que uma cotação é excluída, o `QuoteDeleted` método é chamado no agente de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-168">Each time a quote is deleted, the `QuoteDeleted` method is called on the logger.</span></span> <span data-ttu-id="1d0fd-169">Um escopo de log é adicionado a essas mensagens de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-169">A log scope is added to these log messages.</span></span>

<span data-ttu-id="1d0fd-170">Habilitar `IncludeScopes` nas opções de agente de log de console:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-170">Enable `IncludeScopes` in the console logger options:</span></span>

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="1d0fd-171">Configuração `IncludeScopes` é necessária em aplicativos do ASP.NET Core 2.0 para habilitar o log escopos.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-171">Setting `IncludeScopes` is required in ASP.NET Core 2.0 apps to enable log scopes.</span></span> <span data-ttu-id="1d0fd-172">Configuração `IncludeScopes` via *appsettings* arquivos de configuração é um recurso que foi planejado para a versão 2.1 do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-172">Setting `IncludeScopes` via *appsettings* configuration files is a feature that's planned for the ASP.NET Core 2.1 release.</span></span>

<span data-ttu-id="1d0fd-173">O aplicativo de exemplo limpa outros provedores e adiciona filtros para reduzir a saída de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-173">The sample app clears other providers and adds filters to reduce the logging output.</span></span> <span data-ttu-id="1d0fd-174">Isso torna mais fácil ver mensagens de log de exemplo que demonstram `LoggerMessage` recursos.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-174">This makes it easier to see the sample's log messages that demonstrate `LoggerMessage` features.</span></span>

<span data-ttu-id="1d0fd-175">Para criar um escopo de log, adicione um campo para manter um `Func` delegar para o escopo.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-175">To create a log scope, add a field to hold a `Func` delegate for the scope.</span></span> <span data-ttu-id="1d0fd-176">O aplicativo de exemplo cria um campo chamado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1d0fd-176">The sample app creates a field called `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

<span data-ttu-id="1d0fd-177">Use `DefineScope` para criar o delegado.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-177">Use `DefineScope` to create the delegate.</span></span> <span data-ttu-id="1d0fd-178">Até três tipos podem ser especificados para uso como argumentos de modelo quando o delegado é invocado.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-178">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="1d0fd-179">O aplicativo de exemplo usa um modelo de mensagem que inclui o número de aspas excluídas (uma `int` tipo):</span><span class="sxs-lookup"><span data-stu-id="1d0fd-179">The sample app uses a message template that includes the number of deleted quotes (an `int` type):</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

<span data-ttu-id="1d0fd-180">Fornece um método de extensão estático para a mensagem de log.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-180">Provide a static extension method for the log message.</span></span> <span data-ttu-id="1d0fd-181">Inclui quaisquer parâmetros de tipo para propriedades nomeadas aparecem no modelo de mensagem.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-181">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="1d0fd-182">O aplicativo de exemplo usa um `count` de aspas para excluir e retorna `_allQuotesDeletedScope`:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-182">The sample app takes in a `count` of quotes to delete and returns `_allQuotesDeletedScope`:</span></span>

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

<span data-ttu-id="1d0fd-183">A disposição de escopo chama a extensão de log em um `using` bloco:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-183">The scope wraps the logging extension calls in a `using` block:</span></span>

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

<span data-ttu-id="1d0fd-184">Inspecione as mensagens de log de saída do console do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1d0fd-184">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="1d0fd-185">O resultado a seguir mostra três aspas excluídas com a mensagem de escopo de log incluída:</span><span class="sxs-lookup"><span data-stu-id="1d0fd-185">The following result shows three quotes deleted with the log scope message included:</span></span>

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

## <a name="see-also"></a><span data-ttu-id="1d0fd-186">Consulte também</span><span class="sxs-lookup"><span data-stu-id="1d0fd-186">See also</span></span>

* [<span data-ttu-id="1d0fd-187">Registro em log</span><span class="sxs-lookup"><span data-stu-id="1d0fd-187">Logging</span></span>](xref:fundamentals/logging/index)
