---
title: "Registro em log no núcleo do ASP.NET"
author: ardalis
description: "Apresenta a estrutura de registro do ASP.NET Core. Inclui uma seção para cada provedor de logs interno e links para alguns provedores de terceiros populares."
keywords: Os escopos do ASP.NET Core, registro em log, provedores de log, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, log de eventos, EventSource,
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30e00e2a442225bbe04be0d343f7048efe484477
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Introdução ao registro em log no núcleo do ASP.NET

Por [Steve Smith](http://ardalis.com) e [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de log. Provedores internos permitem que você envie logs para um ou mais destinos, e você pode conectar uma estrutura de log de terceiros. Este artigo mostra como usar a API de registro em log internos e provedores em seu código.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a>Como criar logs

Para criar logs, obter um `ILogger` de objeto o [injeção de dependência](dependency-injection.md) contêiner:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Em seguida, chame métodos de registro em log nesse objeto de agente de log:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Este exemplo cria logs com o `TodoController` classe como o *categoria*.  Categorias são explicadas [posteriormente neste artigo](#log-category).

ASP.NET Core não fornece async métodos do agente porque o log deve ser tão rápido que ela não vale a pena o custo do uso de async. Se você estiver em uma situação em que não é true, considere alterar a maneira de que fazer logon.  Se o repositório de dados estiver lento, gravar as mensagens de log em um repositório rápido primeiro e movê-los para um repositório lenta mais tarde. Por exemplo, faça uma fila de mensagens que é lida e persistida para armazenamento lento por outro processo.

## <a name="how-to-add-providers"></a>Como adicionar provedores

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Um provedor de log leva as mensagens que você criar com um `ILogger` do objeto, exibe e armazena-os. Por exemplo, o provedor de Console exibe mensagens no console, e o provedor de serviço de aplicativo do Azure pode armazená-los no armazenamento de BLOBs do Azure.

Para usar um provedor, chamar o provedor `Add<ProviderName>` método de extensão no *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

O modelo de projeto padrão define o registro em log da maneira que você vê-lo no código anterior, mas o `ConfigureLogging` chamada é feita pelo `CreateDefaultBuilder` método. Aqui está o código em *Program.cs* que são criadas por modelos de projeto:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Um provedor de log leva as mensagens que você criar com um `ILogger` do objeto, exibe e armazena-os. Por exemplo, o provedor de Console exibe mensagens no console, e o provedor de serviço de aplicativo do Azure pode armazená-los no armazenamento de BLOBs do Azure.

Para usar um provedor, instale o pacote do NuGet e chame o método de extensão do provedor em uma instância do `ILoggerFactory`, conforme mostrado no exemplo a seguir.

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [injeção de dependência](dependency-injection.md) (DI) fornece o `ILoggerFactory` instância. O `AddConsole` e `AddDebug` métodos de extensão são definidos no [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) pacotes. Cada método de extensão chama o `ILoggerFactory.AddProvider` método, passando uma instância do provedor. 

> [!NOTE]
> O aplicativo de exemplo para este artigo adiciona provedores de log no `Configure` método o `Startup` classe. Se você quiser obter saída de log do código que é executado anteriormente, adicionar provedores de log no `Startup` em vez disso, o construtor de classe. 

---

Você encontrará informações sobre cada [provedor de logs interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) posteriormente no artigo.

## <a name="sample-logging-output"></a>Exemplo de saída de log

Com o código de exemplo mostrado na seção anterior, você verá logs no console quando executado na linha de comando. Aqui está um exemplo de saída do console:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
Esses logs criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução de ambos `ILogger` chamadas mostradas na seção anterior.

Aqui está um exemplo de como os mesmos logs que aparecem na janela de depuração quando você executar o aplicativo de exemplo no Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Os logs criados pelo `ILogger` chamadas mostradas na seção anterior começam com "TodoApi.Controllers.TodoController". Os logs que começam com categorias de "Microsoft" são do ASP.NET Core. ASP.NET Core em si e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.

O restante deste artigo explica alguns detalhes e as opções de log.

## <a name="nuget-packages"></a>Pacotes NuGet

O `ILogger` e `ILoggerFactory` interfaces estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), e implementações padrão para que eles estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Categoria de log

Um *categoria* é incluído com cada log que você criar.  Especificar a categoria quando você cria um `ILogger` objeto. A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados.  Por exemplo: "TodoApi.Controllers.TodoController".

Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que é derivado da categoria do tipo. Para especificar a categoria como uma cadeia de caracteres, chamar `CreateLogger` em um `ILoggerFactory` de instância, conforme mostrado abaixo.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Na maioria das vezes, é mais fácil de usar `ILogger<T>`, conforme mostrado no exemplo a seguir.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado do `T`.

## <a name="log-level"></a>Nível de log

Cada vez que você gravar um log, especifique seu [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). O nível de log indica o grau de severidade ou importância.  Por exemplo, você pode escrever um `Information` registrar quando um método é finalizado normalmente, um `Warning` quando um método retorna o código de retorno 404 e um log `Error` log quando você captura uma exceção inesperada.

No seguinte exemplo de código, os nomes dos métodos (por exemplo, `LogWarning`) Especifique o nível de log.  O primeiro parâmetro é o [ID de evento de Log](#log-event-id) (explicado mais adiante neste artigo).  Os parâmetros restantes construir uma cadeia de caracteres de mensagem de log.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Métodos de log que incluem o nível no nome do método estão [métodos de extensão para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). Nos bastidores, chamam esses métodos um `Log` método que utiliza um `LogLevel` parâmetro. Você pode chamar o `Log` método diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicado. Para obter mais informações, consulte o [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e [código-fonte extensões do agente de log](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core define os seguintes [níveis de log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordenados aqui de severidade mais alta para menos.

* Rastreamento = 0

  Para obter informações valiosas somente para um desenvolvedor de depurar um problema. Essas mensagens podem conter dados confidenciais de aplicativos e portanto não devem ser habilitadas em um ambiente de produção. *Desabilitado por padrão.* Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Depurar = 1

  Para obter informações que não tem utilidade de curto prazo durante o desenvolvimento e depuração. Exemplo: `Entering method Configure with flag set to true.` você normalmente não permite `Debug` nível registra em produção, a menos que você estiver solucionando problemas, devido ao alto volume de logs.

* Informações = 2

  Para rastrear o fluxo geral do aplicativo. Esses logs normalmente têm algum valor de longo prazo. Exemplo: `Request received for path /api/todo`

* Aviso = 3

  Para eventos anormais ou inesperados no fluxo do aplicativo. Eles podem incluir erros ou outras condições que fazem com que o aplicativo pare, mas que talvez precise ser investigado. Exceções manipuladas estão um local comum para usar o `Warning` nível de log. Exemplo: `FileNotFoundException for file quotes.txt.`

* Erro = 4

  Para erros e exceções que não podem ser manipuladas. Essas mensagens indicam uma falha na operação (como a solicitação HTTP atual) ou a atividade atual, não é uma falha de todo o aplicativo. Exemplo de mensagem de log:`Cannot insert record due to duplicate key violation.`

* Crítico = 5

  Falhas que exigem atenção imediata. Exemplos: dados cenários de perda, espaço em disco insuficiente.

Você pode usar o nível de log para controlar quanto saída de log é gravada em uma mídia de armazenamento específico ou exibir a janela. Por exemplo, em produção, você pode querer todos os logs de `Information` nível e para baixo para ir para um repositório de dados de volume e todos os logs de `Warning` nível e superior para ir para um repositório de dados de valor. Durante o desenvolvimento, você normalmente pode enviar logs de `Warning` ou severidade mais alta para o console. Em seguida, quando você precisar solucionar problemas, você pode adicionar `Debug` nível. O [filtragem de Log](#log-filtering) seção mais adiante neste artigo explica como controlar os níveis de log que trata de um provedor.

Grava a estrutura do ASP.NET Core `Debug` nível logs de eventos do framework. Os exemplos de log anteriormente neste artigo abaixo de logs excluído `Information` nível, portanto, não há `Debug` logs níveis foram mostrados. Aqui está um exemplo de logs do console, se você executar o aplicativo de exemplo configurado para mostrar `Debug` e logs mais alto para o provedor de console.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>ID de evento de log

Cada vez que você grava um log, você pode especificar um *ID do evento*. O aplicativo de exemplo faz isso usando um definidos localmente `LoggingEvents` classe:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Uma ID de evento é um valor inteiro que você pode usar para associar um conjunto de eventos registrados entre si. Por exemplo, um log para adicionar um item a um carrinho de compras pode ser a identificação de evento 1000 e um log para concluir uma compra pode ser a identificação de evento 1001.

Na saída de log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor.  O provedor de depuração não mostra as identificações de evento, mas o provedor do console mostra-los entre colchetes após a categoria:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>Cadeia de formato de mensagem de log

Cada vez que você gravar um log, em que você fornecer uma mensagem de texto. A cadeia de caracteres de mensagem pode conter espaços reservados nomeados para o argumento de valores são inseridos, como no exemplo a seguir:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

A ordem dos espaços reservados, não seus nomes, determina quais parâmetros são usados para eles. Por exemplo, se você tiver o seguinte código:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

A mensagem de log resultante teria esta aparência:

```
Parameter values: parm1, parm2
```

Estrutura de registros de mensagens formatação dessa maneira para possibilitar que os provedores de log implementar [log semântico, também conhecido como registro em log estruturado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Como os próprios argumentos são passados para o sistema de log, não apenas a mensagem formatada cadeia de caracteres, provedores de log podem armazenar os valores de parâmetro como campos além de cadeia de caracteres de mensagem. Por exemplo, se você estiver direcionando o log de saída para o armazenamento de tabela do Azure e sua chamada de método do agente de log tem esta aparência:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Cada entidade de tabela do Azure pode ter `ID` e `RequestTime` propriedades, que seriam simplificam as consultas em dados de log. Você pode encontrar todos os logs em um determinado `RequestTime` intervalo, sem a necessidade de analisar o tempo limite da mensagem de texto.

## <a name="logging-exceptions"></a>Exceções de registro em log

Os métodos de agente de log têm sobrecargas que permitem que você passar uma exceção, como no exemplo a seguir:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Provedores diferentes lidar com as informações de exceção de maneiras diferentes. Aqui está um exemplo da saída do provedor de depuração de código mostrado acima.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtragem de log

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Você pode especificar um nível de log mínimo para um provedor específico e uma categoria ou para todos os provedores ou todas as categorias.  Logs abaixo do nível mínimo não são passados para esse provedor, para que eles não obter exibidos ou armazenados. 

Se você desejar suprimir todos os logs, você pode especificar `LogLevel.None` como o nível de log mínimo. O valor inteiro do `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).

**Criar regras de filtro na configuração**

Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o log para os provedores de Console e de depuração. O `CreateDefaultBuilder` método também define o registro em log para procurar a configuração em um `Logging` seção, usando código semelhante ao seguinte:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Os dados de configuração especificam os níveis de log mínimo pelo provedor e a categoria, como no exemplo a seguir:

[!code-json[](logging/sample2/appsettings.json)]

Este JSON cria seis regras de filtro, um para o provedor de depuração, quatro para o provedor de Console e que se aplica a todos os provedores. Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um `ILogger` objeto é criado.

**Regras de filtro no código**

Você pode registrar as regras de filtro no código, conforme mostrado no exemplo a seguir:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

O segundo `AddFilter` Especifica o provedor de depuração usando seu nome de tipo. A primeira `AddFilter` se aplica a todos os provedores porque ela não especifica um tipo de provedor.

**Como filtrar as regras são aplicadas**

Os dados de configuração e o `AddFilter` código mostrado nos exemplos anteriores criar as regras mostradas na tabela a seguir. Primeiro seis vêm de exemplo de configuração e os dois últimos vêm do exemplo de código.

Número|Provider|Categorias que começam com|Nível de log mínimo|
------|--------|--------------------------|-----------------|
1|Depurar|Todas as categorias|Informações|
2|Console|Microsoft.AspNetCore.Mvc.Razor.Internal|Aviso|
3|Console|Microsoft.AspNetCore.Mvc.Razor.Razor|Depurar|
4|Console|Microsoft.AspNetCore.Mvc.Razor|Erro|
5|Console|Todas as categorias|Informações|
6|Todos os provedores|Todas as categorias|Aviso
7|Todos os provedores|Sistema|Depurar
8|Depurar|Microsoft|rastreamento

Quando você cria um `ILogger` objeto gravar logs com o `ILoggerFactory` objeto seleciona uma única regra por provedor para aplicar a esse agente. Todas as mensagens gravadas pelo `ILogger` objeto são filtrados com base nas regras selecionadas. A regra mais específica possível para cada par de categoria e o provedor é selecionada de regras disponíveis.

O seguinte algoritmo é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:

* Selecione todas as regras que correspondem ao provedor ou seu alias.  Se nenhum for encontrado, selecione todas as regras com um provedor vazio.
* Do resultado da etapa anterior, selecione as regras com maior correspondência de prefixo de categoria. Se nenhum for encontrado, selecione todas as regras que não especificar uma categoria.
* Se várias regras forem selecionadas terão o **último** um.
* Se nenhuma regra estiver selecionada, use `MinimumLevel`.
 
Por exemplo, suponha que você tem a lista anterior de regras e criar um `ILogger` objeto para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Para o provedor de depuração, 1, 6 e 8 as regras se aplicam. Regra de 8 é o mais específica, para que seja selecionado.
* Para o provedor de Console, 3, 4, 5 e 6 de regras se aplicam. A regra 3 é o mais específica.

Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de `Trace` nível e acima para ir para o provedor de depuração e logs de `Debug` nível e acima para ir para o provedor de Console.

**Aliases de provedor**

Você pode usar o nome do tipo para especificar um provedor de configuração, mas cada provedor define um menor *alias* que é mais fácil de usar. Para os provedores internos, use os seguintes aliases:

- Console
- Depurar
- Log de eventos
- AzureAppServices
- TraceSource
- EventSource

**Nível mínimo de padrão**

Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração aplicáveis para um determinado provedor e uma categoria. O exemplo a seguir mostra como definir o nível mínimo:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Se você não definir explicitamente o nível mínimo, o valor padrão é `Information`, o que significa que `Trace` e `Debug` logs são ignorados.

**Funções de filtro**

Você pode escrever código em uma função de filtro para aplicar regras de filtragem. Uma função de filtro é invocada para todos os provedores e as categorias que não têm regras atribuídas a eles por configuração ou código. O código na função tem acesso para o tipo de provedor, a categoria e o nível de log para decidir se uma mensagem deve ser registrada. Por exemplo:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados com base no nível de log e categoria.

O `AddConsole` e `AddDebug` métodos de extensão fornecem sobrecargas que permitem que você passe em critérios de filtragem. O código de exemplo a seguir faz com que o provedor de console ignorar logs abaixo `Warning` nível, enquanto o provedor de depuração ignora os logs que cria a estrutura.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

O `AddEventLog` método tem uma sobrecarga que usa um `EventLogSettings` instância, que pode conter uma função de filtragem no seu `Filter` propriedade. O provedor TraceSource não fornece qualquer uma dessas sobrecargas, porque seu nível de log e outros parâmetros são baseados no `SourceSwitch` e `TraceListener` usa.

Você pode definir regras de filtragem para todos os provedores registrados com um `ILoggerFactory` instância usando o `WithFilter` método de extensão. O exemplo a seguir limita os logs de framework (categoria começa com "Microsoft" ou "Sistema"), os avisos ao mesmo tempo, permitindo que o log de aplicativo no nível de depuração.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Se você quiser usar a filtragem para impedir que todos os logs sejam gravados para uma determinada categoria, você pode especificar `LogLevel.None` como o nível de log mínimo dessa categoria. O valor inteiro do `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).

O `WithFilter` fornecido pelo método de extensão de [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) pacote NuGet. O método retorna um novo `ILoggerFactory` instância que filtra as mensagens de log passadas para todos os provedores de agente registrados com ele. Ele não afeta nenhum outro `ILoggerFactory` instâncias, incluindo original `ILoggerFactory` instância.

---

## <a name="log-scopes"></a>Escopos de log

Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados para cada log que é criado como parte desse conjunto.  Por exemplo, convém cada log criado como parte do processamento de uma transação para incluir a ID da transação.

Um escopo é um `IDisposable` tipo retornado pelo `ILogger.BeginScope<TState>` método e permanece assim até que seja descartada. Use um escopo por encapsulamento chama seu agente de log em um `using` bloquear, conforme mostrado aqui:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

O código a seguir habilita os escopos para o provedor de console:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

Em *Program.cs*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

Em *Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Cada mensagem de log inclui as informações com escopo definido:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Provedores de registro em log internos

ASP.NET Core vem com os seguintes provedores:

* [Console](#console)
* [Depurar](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Serviço de Aplicativo do Azure](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>O provedor de console

O [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pacote provedor envia a saída de log para o console. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[Sobrecargas de AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você transmitir um um nível de log mínimo, uma função de filtro e um valor booleano que indica se há suporte para escopos.  Outra opção é passar um `IConfiguration` objeto, que pode especificar níveis de log e suporte de escopos. 

Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.

Quando você cria um novo projeto no Visual Studio, o `AddConsole` método tem esta aparência:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Esse código se refere a `Logging` seção o *appSettings. JSON* arquivo:

[!code-json[](logging/sample//appsettings.json)]

As configurações mostradas limite framework logs avisos ao mesmo tempo, permitindo que o aplicativo para fazer logon no nível de depuração, conforme explicado no [filtragem de Log](#log-filtering) seção. Para obter mais informações, consulte [configuração](configuration.md).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>O provedor de depuração

O [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pacote provedor grava a saída de log usando o [Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) classe (`Debug.WriteLine` chamadas de método).

No Linux, esse provedor grava logs no *logs /var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[Sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passa um nível de log mínimo ou uma função de filtro.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>O provedor de EventSource

Para aplicativos que se destinam a ASP.NET Core 1.1.0 ou superior, o [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pacote provedor pode implementar o rastreamento de eventos. No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). O provedor é entre plataformas, mas não existem ferramentas de coleta e a exibição para Linux ou macOS em nenhum evento. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Uma boa maneira de coletar e exibir logs é usar o [PerfView utilitário](https://www.microsoft.com/download/details.aspx?id=28567). Há outras ferramentas para exibir os logs do ETW, mas PerfView proporciona a melhor experiência para trabalhar com os eventos ETW emitidos pelo ASP.NET. 

Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` para o **provedores adicionais** lista. (Não perca o asterisco no início da cadeia de caracteres).

![Outros provedores de Perfview](logging/_static/perfview-additional-providers.png)

Capturando eventos em Nano Server requer alguma configuração adicional:

* Conecte-se de comunicação remota do PowerShell para o Nano Server:

  ```powershell
  Enter-PSSession [name]
  ```

* Crie uma sessão do ETW:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Adicionar provedores do ETW para [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core e outros conforme necessário. O provedor ASP.NET Core GUID é `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Executar o site e fazer quaisquer ações que você deseja informações de rastreamento.

* Pare a sessão de rastreamento quando tiver terminado:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Resultante *C:\trace.etl* arquivo pode ser analisado com PerfView como em outras edições do Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>O provedor de log de eventos do Windows

O [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) pacote provedor envia a saída do log no Log de eventos do Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[Sobrecargas de AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você transmitir `EventLogSettings` ou um nível de log mínimo.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>O provedor de TraceSource

O [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provedor pacote usa a [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) bibliotecas e provedores.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[Sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passa uma alternância de origem e um ouvinte de rastreamento.

Para usar esse provedor, um aplicativo deve ser executado no .NET Framework (em vez de .NET Core). O provedor permite rotear mensagens para uma variedade de [ouvintes](https://msdn.microsoft.com/library/4y5y10s7), como o [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) usados no aplicativo de exemplo.

O exemplo a seguir configura um `TraceSource` provedor que registra `Warning` e superior mensagens na janela de console.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>O provedor de serviço de aplicativo do Azure

O [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) pacote provedor grava logs para arquivos de texto no sistema de arquivos do aplicativo do serviço de aplicativo do Azure e ao [armazenamento de blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de armazenamento do Azure. O provedor está disponível apenas para aplicativos que se destinam a ASP.NET Core 1.1.0 ou superior. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2. x](#tab/aspnetcore2x)

> [!NOTE]
> Núcleo do ASP.NET 2.0 está em visualização.  Os aplicativos criados com a versão de visualização mais recente podem não ser executado quando implantado em um serviço de aplicativo do Azure. Quando o ASP.NET Core 2.0 foi lançado, o serviço de aplicativo do Azure executará 2.0 aplicativos e o serviço de aplicativo do Azure provedor funcionará conforme o indicado aqui.

Você não precisa instalar o pacote de provedor ou a chamada a `AddAzureWebAppDiagnostics` método de extensão.  O provedor está automaticamente disponível para seu aplicativo quando você implanta o aplicativo do serviço de aplicativo do Azure.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1. x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Um `AddAzureWebAppDiagnostics` sobrecarga permite que você passe [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), com a qual você pode substituir as configurações padrão como o modelo de saída de log, o nome de blob e o limite de tamanho de arquivo. (*Modelo saída* é uma cadeia de caracteres de formato de mensagem que é aplicada a todos os logs, na parte superior que você fornece ao chamar um `ILogger` método.)  

---

Quando você implanta um aplicativo de serviço de aplicativo, seu aplicativo respeita as configurações a [Logs de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) seção o **do serviço de aplicativo** página do portal do Azure. Quando você alterar essas configurações, as alterações entram em vigor imediatamente sem exigir que você reinicie o aplicativo ou reimplantar o código para ele. 

![Configurações de log do Azure](logging/_static/azure-logging-settings.png)

O local padrão para arquivos de log é o *unidade d:\\inicial\\LogFiles\\aplicativo* pasta e o nome de arquivo padrão é *yyyymmdd.txt diagnóstico*. O limite de tamanho de arquivo padrão é 10 MB, e o número de máximo padrão de arquivos mantidos é 2. O nome de blob padrão é *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

O provedor funciona somente quando o projeto é executado no ambiente do Azure.  Ele não tem nenhum efeito quando é executado localmente &mdash; não gravará arquivos locais ou armazenamento de desenvolvimento local para blobs.

## <a name="third-party-logging-providers"></a>Provedores de log de terceiros

Aqui estão algumas estruturas de registro em log de terceiros que funcionam com o ASP.NET Core:

* [ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -provedor para o serviço Elmah.Io

* [JSNLog](http://jsnlog.com) -registra em log as exceções de JavaScript e outros eventos do lado do cliente no registro do servidor.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -provedor para o serviço Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -provedor para a biblioteca de NLog

* [Serilog](https://github.com/serilog/serilog-framework-logging) -provedor para a biblioteca de Serilog

Algumas estruturas de terceiros podem fazer [log semântico, também conhecido como registro em log estruturado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Usando uma estrutura de terceiros é semelhante ao uso de um dos provedores internos: adicionar um pacote NuGet ao seu projeto e chamar um método de extensão em `ILoggerFactory`. Para obter mais informações, consulte a documentação de cada estrutura.

Você pode criar seus próprios provedores personalizados, para dar suporte a outras estruturas de registro em log ou seus próprios requisitos de registro em log.
