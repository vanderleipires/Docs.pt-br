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
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>Registro em log de alto desempenho com LoggerMessage no núcleo do ASP.NET

Por [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) recursos criam armazenável delegados que exigem menos alocações de objeto e reduziu a sobrecarga de computação que [métodos de extensão do agente de log](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), como `LogInformation`, `LogDebug`e `LogError`. Para cenários de registro em log de alto desempenho, use o `LoggerMessage` padrão.

`LoggerMessage`Fornece as seguintes vantagens de desempenho sobre métodos de extensão do agente de log:

* Métodos de extensão do agente de log exigem tipos de valor de "boxing" (converter), como `int`, em `object`. O `LoggerMessage` padrão evita conversão boxing usando estático `Action` campos e métodos de extensão com parâmetros fortemente tipados.
* Métodos de extensão do agente devem analisar o modelo de mensagem (cadeia de caracteres de formato nomeado) toda vez que uma mensagem de log é gravada. `LoggerMessage`exige apenas analisar um modelo uma vez quando a mensagem é definida.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O aplicativo de exemplo demonstra `LoggerMessage` recursos com aspas básica sistema de controle. O aplicativo adiciona e exclui aspas usando um banco de dados na memória. Como essas operações, mensagens de log são geradas usando o `LoggerMessage` padrão.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[Definir (LogLevel, EventId, String)](/dotnet/api/microsoft.extensions.logging.loggermessage.define) cria um `Action` delegar para registrar uma mensagem. `Define`sobrecargas permitem passar até seis parâmetros de tipo para uma cadeia de caracteres de formato nomeado (modelo).

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) cria um `Func` delegar para definir um [escopo de log](xref:fundamentals/logging/index#log-scopes). `DefineScope`sobrecargas permitem passar até três parâmetros de tipo para uma cadeia de caracteres de formato nomeado (modelo).

## <a name="message-template-named-format-string"></a>Modelo de mensagem (chamado de cadeia de caracteres de formato)

A cadeia de caracteres fornecida para o `Define` e `DefineScope` métodos é um modelo e não uma cadeia de caracteres interpolada. Espaços reservados são preenchidos na ordem em que os tipos são especificados. Nomes de espaço reservado no modelo devem ser descritivo e consistente em modelos. Eles servem como nomes de propriedade em dados estruturados de log. É recomendável [Pascal maiusculas e minúsculas](/dotnet/standard/design-guidelines/capitalization-conventions) para nomes de espaço reservado. Por exemplo, `{Count}`, `{FirstName}`.

## <a name="implementing-loggermessagedefine"></a>Implementando LoggerMessage.Define

Cada mensagem de log é um `Action` mantido em um campo estático criado por `LoggerMessage.Define`. Por exemplo, o aplicativo de exemplo cria um campo para descrever uma mensagem de log para uma solicitação GET para a página de índice (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

Para o `Action`, especifique:

* O nível de log.
* Um identificador exclusivo do evento ([EventId](/dotnet/api/microsoft.extensions.logging.eventid)) com o nome do método de extensão estático.
* O modelo de mensagem (chamado de cadeia de caracteres de formato). 

Uma solicitação para a página de índice dos conjuntos de aplicativo de exemplo a:

* Nível de log `Information`.
* Id do evento para `1` com o nome do `IndexPageRequested` método.
* Modelo de mensagem (chamado de cadeia de caracteres de formato) para uma cadeia de caracteres.

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Repositórios de log estruturado podem usar o nome do evento quando ele é fornecido com a id de evento para enriquecer o registro em log. Por exemplo, [Serilog](https://github.com/serilog/serilog-extensions-logging) usa o nome do evento.

O `Action` é invocado por meio de um método de extensão fortemente tipada. O `IndexPageRequested` método registra uma mensagem para uma solicitação de obtenção de página de índice no aplicativo de exemplo:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested`é chamado no agente de log no `OnGetAsync` método *Pages/Index.cshtml.cs*:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Inspecione a saída do console do aplicativo:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Para passar parâmetros para uma mensagem de log, defina até seis tipos ao criar o campo estático. O aplicativo de exemplo registra uma cadeia de caracteres ao adicionar uma cotação definindo um `string` de tipo para o `Action` campo:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Modelo de mensagem de log do representante recebe seus valores de espaço reservado de tipos fornecidos. O aplicativo de exemplo define um delegado para adicionar uma cotação onde o parâmetro de cotação é um `string`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

O método de extensão estático para adicionar uma cotação, `QuoteAdded`, recebe o valor do argumento aspas e passa para o `Action` delegar:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

No arquivo de code-behind da página de índice (*Pages/Index.cshtml.cs*), `QuoteAdded` é chamado para registrar a mensagem:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Inspecione a saída do console do aplicativo:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

A exemplo aplicativo implementa uma `try` &ndash; `catch` padrão para exclusão de aspas. Uma mensagem informativa é registrada para uma operação de exclusão com êxito. Uma mensagem de erro é registrada para uma operação de exclusão quando uma exceção será lançada. A mensagem de log para a operação de exclusão de êxito inclui o rastreamento de pilha de exceção (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Observe como a exceção é passada para o representante em `QuoteDeleteFailed`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

No índice página code-behind, a exclusão bem-sucedida de cotação chama o `QuoteDeleted` método no agente de log. Quando uma cotação não foi encontrada para exclusão, um `ArgumentNullException` é gerada. A exceção é interceptada pelo `try` &ndash; `catch` instrução e conectado ao chamar o `QuoteDeleteFailed` método no agente de log no `catch` bloco (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Quando uma cotação é excluída com êxito, verifique a saída do console do aplicativo:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Quando ocorre falha na exclusão de cotação, inspecione a saída do console do aplicativo. Observe que a exceção é incluída na mensagem de log:

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

## <a name="implementing-loggermessagedefinescope"></a>Implementando LoggerMessage.DefineScope

Definir um [escopo de log](xref:fundamentals/logging/index#log-scopes) para aplicar a uma série de mensagens de log usando o [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) método.

O aplicativo de exemplo tem uma **Limpar tudo** botão para excluir todas as aspas no banco de dados. As aspas são excluídas, removendo-os um por vez. Cada vez que uma cotação é excluída, o `QuoteDeleted` método é chamado no agente de log. Um escopo de log é adicionado a essas mensagens de log.

Habilitar `IncludeScopes` nas opções de agente de log de console:

[!code-csharp[Main](loggermessage/sample/Program.cs?name=snippet1&highlight=22)]

Configuração `IncludeScopes` é necessária em aplicativos do ASP.NET Core 2.0 para habilitar o log escopos. Configuração `IncludeScopes` via *appsettings* arquivos de configuração é um recurso que foi planejado para a versão 2.1 do ASP.NET Core.

O aplicativo de exemplo limpa outros provedores e adiciona filtros para reduzir a saída de log. Isso torna mais fácil ver mensagens de log de exemplo que demonstram `LoggerMessage` recursos.

Para criar um escopo de log, adicione um campo para manter um `Func` delegar para o escopo. O aplicativo de exemplo cria um campo chamado `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Use `DefineScope` para criar o delegado. Até três tipos podem ser especificados para uso como argumentos de modelo quando o delegado é invocado. O aplicativo de exemplo usa um modelo de mensagem que inclui o número de aspas excluídas (uma `int` tipo):

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Fornece um método de extensão estático para a mensagem de log. Inclui quaisquer parâmetros de tipo para propriedades nomeadas aparecem no modelo de mensagem. O aplicativo de exemplo usa um `count` de aspas para excluir e retorna `_allQuotesDeletedScope`:

[!code-csharp[Main](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

A disposição de escopo chama a extensão de log em um `using` bloco:

[!code-csharp[Main](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Inspecione as mensagens de log de saída do console do aplicativo. O resultado a seguir mostra três aspas excluídas com a mensagem de escopo de log incluída:

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

## <a name="see-also"></a>Consulte também

* [Registro em log](xref:fundamentals/logging/index)
