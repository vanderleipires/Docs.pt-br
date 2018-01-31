---
title: Registro em log no ASP.NET Core
author: ardalis
description: Saiba mais sobre a estrutura de registros no ASP.NET Core. Descubra os provedores de log internos e saiba mais sobre os provedores de terceiros populares.
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: c8152b94311acb672e9810828b634c744cb46eae
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Introdução ao registro em log no ASP.NET Core

Por [Steve Smith](https://ardalis.com/) e [Tom Dykstra](https://github.com/tdykstra)

O ASP.NET Core dá suporte a uma API de registro em log que funciona com uma variedade de provedores de logs. Os provedores internos permitem que você envie logs para um ou mais destinos e você pode se conectar a uma estrutura de registros de terceiros. Este artigo mostra como usar a API de registro em log e os provedores internos em seu código.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="how-to-create-logs"></a>Como criar logs

Para criar logs, obtenha um objeto `ILogger` do contêiner de [injeção de dependência](xref:fundamentals/dependency-injection):

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Em seguida, chame métodos de registro em log nesse objeto de agente:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Este exemplo cria logs com a classe `TodoController` como a *categoria*. As categorias serão explicadas [posteriormente neste artigo](#log-category).

O ASP.NET Core não fornece métodos de agente assíncronos porque o registro em log deve ser tão rápido que não valeria a pena usar assíncronos. Se você estiver em uma situação em que isso não é verdadeiro, considere alterar a maneira como você faz logs. Se seu armazenamento de dados é lento, grave as mensagens de log em um repositório rápido primeiro e, depois, mova-as para um repositório lento. Por exemplo, registre em uma fila de mensagens que seja lida e persistida em um armazenamento lento por outro processo.

## <a name="how-to-add-providers"></a>Como adicionar provedores

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena. Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.

Para usar um provedor, chame o método de extensão `Add<ProviderName>` do provedor no *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

O modelo de projeto padrão permite o registro em log com o método [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Um provedor de logs obtém as mensagens que você cria com um objeto `ILogger` e as exibe ou armazena. Por exemplo, o provedor Console exibe as mensagens no console e o provedor do Serviço de Aplicativo do Azure pode armazená-las no armazenamento de blobs do Azure.

Para usar um provedor, instale o pacote NuGet e chame o método de extensão do provedor em uma instância de `ILoggerFactory`, conforme mostrado no exemplo a seguir.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

A [DI](xref:fundamentals/dependency-injection) (injeção de dependência) do ASP.NET Core fornece a instância de `ILoggerFactory`. Os métodos de extensão `AddConsole` e `AddDebug` são definidos nos pacotes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) e [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Cada método de extensão chama o método `ILoggerFactory.AddProvider`, passando uma instância do provedor. 

> [!NOTE]
> O aplicativo de exemplo deste artigo adiciona provedores de log no método `Configure` da classe `Startup`. Se você quiser obter saída de log de código que é executado anteriormente, adicione provedores de log no construtor de classe `Startup`. 

---

Você encontrará informações sobre cada [provedor de log interno](#built-in-logging-providers) e links para [provedores de log de terceiros](#third-party-logging-providers) mais adiante no artigo.

## <a name="sample-logging-output"></a>Exemplo de saída de registro em log

Com o código de exemplo mostrado na seção anterior, você verá logs no console ao executar através da linha de comando. Aqui está um exemplo da saída do console:

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
 
Esses logs foram criados acessando `http://localhost:5000/api/todo/0`, que dispara a execução das duas chamadas `ILogger` mostradas na seção anterior.

Aqui está um exemplo de como os mesmos logs aparecem na janela Depuração quando você executa o aplicativo de exemplo no Visual Studio:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Os logs criados pelas chamadas `ILogger` mostradas na seção anterior começam com "TodoApi.Controllers.TodoController". Os logs que começam com categorias "Microsoft" são do ASP.NET Core. O próprio ASP.NET Core e o código do aplicativo estão usando a mesma API de registro em log e os mesmos provedores de log.

O restante deste artigo explica alguns detalhes e opções para registro em log.

## <a name="nuget-packages"></a>Pacotes NuGet

As interfaces `ILogger` e `ILoggerFactory` estão em [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/) e as implementações padrão para elas estão em [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Categoria de log

Um *categoria* é incluída com cada log que você cria. Você especifica a categoria ao criar um objeto `ILogger`. A categoria pode ser qualquer cadeia de caracteres, mas uma convenção é usar o nome totalmente qualificado da classe da qual os logs são gravados. Por exemplo: "TodoApi.Controllers.TodoController".

Você pode especificar a categoria como uma cadeia de caracteres ou usar um método de extensão que deriva a categoria do tipo. Para especificar a categoria como uma cadeia de caracteres, chame `CreateLogger` em uma instância de `ILoggerFactory`, conforme mostrado abaixo.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Na maioria das vezes será mais fácil usar `ILogger<T>`, conforme mostrado no exemplo a seguir.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Isso é equivalente a chamar `CreateLogger` com o nome de tipo totalmente qualificado de `T`.

## <a name="log-level"></a>Nível de log

Sempre que você grava um log, você especifica o [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). O nível de log indica o grau de gravidade ou importância. Por exemplo, você pode gravar um log `Information` quando um método é finalizado normalmente, um log `Warning` quando um método retorna um código de retorno 404 e um log `Error` ao capturar uma exceção inesperada.

No exemplo de código a seguir, os nomes dos métodos (por exemplo, `LogWarning`) especificam o nível de log. O primeiro parâmetro é a [ID de evento de log](#log-event-id). O segundo parâmetro é um [modelo de mensagem](#log-message-template) com espaços reservados para valores de argumento fornecidos pelos parâmetros de método restantes. Os parâmetros de método serão explicados com mais detalhes posteriormente neste artigo.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Os métodos de log que incluem o nível no nome do método são [métodos de extensão para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). Nos bastidores, esses métodos chamam um método `Log` que recebe um parâmetro `LogLevel`. Você pode chamar o método `Log` diretamente em vez de um desses métodos de extensão, mas a sintaxe é relativamente complicada. Para obter mais informações, consulte a [interface ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) e o [código-fonte de extensões de agente](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

O ASP.NET Core define os seguintes [níveis de log](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordenados aqui da menor para a maior gravidade.

* Trace = 0

  Para obter informações valiosas somente para um desenvolvedor que esteja depurando um problema. Essas mensagens podem conter dados confidenciais de aplicativos e, portanto, não devem ser habilitadas em um ambiente de produção. *Desabilitado por padrão.* Exemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debug = 1

  Para informações que tenham utilidade de curto prazo durante o desenvolvimento e a depuração. Exemplo: `Entering method Configure with flag set to true.` Você normalmente não habilitaria logs de nível `Debug` em produção, a menos que estivesse solucionando problemas, devido ao alto volume de logs.

* Information = 2

  Para rastrear o fluxo geral do aplicativo. Esses logs normalmente têm algum valor a longo prazo. Exemplo: `Request received for path /api/todo`

* Warning = 3

  Para eventos anormais ou inesperados no fluxo de aplicativo. Eles podem incluir erros ou outras condições que não fazem com que o aplicativo pare, mas que talvez precisem ser investigados. Exceções manipuladas são um local comum para usar o nível de log `Warning`. Exemplo: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Para erros e exceções que não podem ser manipulados. Essas mensagens indicam uma falha na atividade ou na operação atual (como a solicitação HTTP atual) e não uma falha em todo o aplicativo. Mensagem de log de exemplo:`Cannot insert record due to duplicate key violation.`

* Critical = 5

  Para falhas que exigem atenção imediata. Exemplos: cenários de perda de dados, espaço em disco insuficiente.

Você pode usar o nível de log para controlar a quantidade de saída de log que é gravada em uma mídia de armazenamento específica ou em uma janela de exibição. Por exemplo, em produção, você pode desejar que todos os logs de nível `Information` e inferior vão para um armazenamento de dados de volume e que todos os logs de nível `Warning` e superior vão para um armazenamento de dados de valor. Durante o desenvolvimento, você normalmente poderia enviar os logs de gravidade `Warning` ou mais alta para o console. Então, quando precisar solucionar problemas, você poderá adicionar o nível `Debug`. A seção [Filtragem de log](#log-filtering) mais adiante neste artigo explicará como controlar os níveis de log que um provedor manipula.

A estrutura do ASP.NET Core grava logs de nível `Debug` para eventos de estrutura. Os exemplos de log anteriores neste artigo excluíram logs abaixo do nível `Information`, portanto, os logs de nível `Debug` não foram mostrados. Aqui está um exemplo de logs do console, caso você execute o aplicativo de exemplo configurado para mostrar logs de nível `Debug` e superiores para o provedor de console.

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

Sempre que você grava um log, você pode especificar uma *ID de evento*. O aplicativo de exemplo faz isso usando uma classe `LoggingEvents` definida localmente:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Uma ID de evento é um valor inteiro que pode ser usado para associar um conjunto de eventos registrados. Por exemplo, um log para adicionar um item a um carrinho de compras pode ser ter a ID de evento 1000 e um log para concluir uma compra pode ter a ID de evento 1001.

Na saída do registro em log, a ID do evento pode ser armazenada em um campo ou incluída na mensagem de texto, dependendo do provedor. O provedor Depuração não mostra as IDs de evento, mas o provedor do console sim, entre colchetes depois da categoria:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Modelo de mensagem de log

Sempre que você grava uma mensagem de log, você fornece um modelo de mensagem. O modelo de mensagem pode ser uma cadeia de caracteres ou pode conter espaços reservados nomeados, nos quais valores de argumento serão colocados. O modelo não é uma cadeia de caracteres de formatação e os espaços reservados devem ser nomeados, não numerados.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

A ordem dos espaços reservados e não de seus nomes, determina quais parâmetros serão usados para fornecer seus valores. Se você tiver o seguinte código:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

A mensagem de log resultante terá esta aparência:

```
Parameter values: parm1, parm2
```

A estrutura de registros realiza a formatação de mensagens dessa maneira para possibilitar que os provedores de log implementem o [registro em log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Como os próprios argumentos são passados para o sistema de registro em log, não apenas o modelo de mensagem formatada, mas os provedores de log, também poderão armazenar os valores de parâmetro como campos, além do modelo de mensagem. Se você estiver direcionando a saída de log para o Armazenamento de Tabelas do Azure e sua chamada de método do agente tiver esta aparência:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Cada entidade da Tabela do Azure poderá ter propriedades `ID` e `RequestTime`, o que simplificará as consultas nos dados de log. Você poderá encontrar todos os logs em um determinado intervalo de `RequestTime` sem a necessidade de analisar o tempo limite da mensagem de texto.

## <a name="logging-exceptions"></a>Exceções de registro em log

Os métodos de agente têm sobrecargas que permitem que você passe uma exceção, como no exemplo a seguir:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Provedores diferentes manipulam as informações de exceção de maneiras diferentes. Aqui está um exemplo da saída do provedor Depuração do código mostrado acima.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtragem de log

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Você pode especificar um nível de log mínimo para um provedor e uma categoria específicos ou para todos os provedores ou todas as categorias. Os logs abaixo do nível mínimo não serão passados para esse provedor, para que não sejam exibidos ou armazenados. 

Se quiser suprimir todos os logs, você poderá especificar `LogLevel.None` como o nível de log mínimo. O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).

**Criar regras de filtro na configuração**

Os modelos de projeto criam código que chama `CreateDefaultBuilder` para configurar o registro em log para os provedores Console e Depuração. O método `CreateDefaultBuilder` também configura o registro em log para procurar a configuração em uma seção `Logging`, usando código semelhante ao seguinte:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Os dados de configuração especificam níveis de log mínimo por provedor e por categoria, como no exemplo a seguir:

[!code-json[](index/sample2/appsettings.json)]

Este JSON cria seis regras de filtro, uma para o provedor Depuração, quatro para o provedor Console e uma que se aplica a todos os provedores. Você verá posteriormente como apenas uma dessas regras é escolhida para cada provedor quando um objeto `ILogger` é criado.

**Regras de filtro no código**

Você pode registrar regras de filtro no código, conforme mostrado no exemplo a seguir:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

O segundo `AddFilter` especifica o provedor Depuração usando seu nome de tipo. O primeiro `AddFilter` se aplica a todos os provedores porque ele não especifica um tipo de provedor.

**Como as regras de filtragem são aplicadas**

Os dados de configuração e o código `AddFilter`, mostrados nos exemplos anteriores, criam as regras mostradas na tabela a seguir. As primeiras seis vêm do exemplo de configuração e as últimas duas vêm do exemplo de código.

| Número | Provider      | Categorias que começam com...          | Nível de log mínimo |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Depurar         | Todas as categorias                          | Informações       |
| 2      | Console       | Microsoft.AspNetCore.Mvc.Razor.Internal | Aviso           |
| 3      | Console       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Depurar             |
| 4      | Console       | Microsoft.AspNetCore.Mvc.Razor          | Erro             |
| 5      | Console       | Todas as categorias                          | Informações       |
| 6      | Todos os provedores | Todas as categorias                          | Depurar             |
| 7      | Todos os provedores | Sistema                                  | Depurar             |
| 8      | Depurar         | Microsoft                               | Rastrear             |

Quando você cria um objeto `ILogger` para usar para gravar logs, o objeto `ILoggerFactory` seleciona uma única regra por provedor para aplicar a esse agente. Todas as mensagens gravadas pelo objeto `ILogger` são filtradas com base nas regras selecionadas. A regra mais específica possível para cada par de categoria e provedor é selecionada dentre as regras disponíveis.

O algoritmo a seguir é usado para cada provedor quando um `ILogger` é criado para uma determinada categoria:

* Selecione todas as regras que correspondem ao provedor ou seu alias. Se nenhuma for encontrada, selecione todas as regras com um provedor vazio.
* Do resultado da etapa anterior, selecione as regras com o prefixo de categoria de maior correspondência. Se nenhuma for encontrada, selecione todas as regras que não especificam uma categoria.
* Se várias regras forem selecionadas use a **última**.
* Se nenhuma regra for selecionada, use `MinimumLevel`.
 
Por exemplo, suponha que você tem a lista anterior de regras e cria um objeto `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Para o provedor Depuração as regras 1, 6 e 8 se aplicam. A regra 8 é mais específica, portanto é a que será selecionada.
* Para o provedor Console as regras 3, 4, 5 e 6 se aplicam. A regra 3 é a mais específica.

Quando você cria logs com um `ILogger` para a categoria "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", os logs de nível `Trace` e superiores vão para o provedor Depuração e os logs de nível `Debug` e superiores vão para o provedor Console.

**Aliases de provedor**

Você pode usar o nome do tipo para especificar um provedor na configuração, mas cada provedor define um *alias* menor que é mais fácil de usar. Para os provedores internos, use os seguintes aliases:

- Console
- Depurar
- EventLog
- AzureAppServices
- TraceSource
- EventSource

**Nível mínimo padrão**

Há uma configuração de nível mínimo que entra em vigor somente se nenhuma regra de código ou de configuração se aplicar a um provedor e uma categoria determinados. O exemplo a seguir mostra como definir o nível mínimo:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Se você não definir explicitamente o nível mínimo, o valor padrão será `Information`, o que significa que logs `Trace` e `Debug` serão ignorados.

**Funções de filtro**

Você pode escrever código em uma função de filtro para aplicar regras de filtragem. Uma função de filtro é invocada para todos os provedores e categorias que não têm regras atribuídas a eles por configuração ou código. O código na função tem acesso ao tipo de provedor, à categoria e ao nível de log para decidir se uma mensagem deve ser registrada. Por exemplo:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Alguns provedores de log permitem especificar quando os logs devem ser gravados em uma mídia de armazenamento ou ignorados, com base no nível de log e na categoria.

Os métodos de extensão `AddConsole` e `AddDebug` fornecem sobrecargas que permitem que você passe critérios de filtragem. O código de exemplo a seguir faz com que o provedor de console ignore os logs abaixo do nível `Warning`, enquanto o provedor Depuração ignora os logs criados pela estrutura.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

O método `AddEventLog` tem uma sobrecarga que recebe uma instância `EventLogSettings`, que pode conter uma função de filtragem em sua propriedade `Filter`. O provedor TraceSource não fornece nenhuma dessas sobrecargas, porque seu nível de registro em log e outros parâmetros são baseados no `SourceSwitch` e no `TraceListener` que ele usa.

Você pode definir regras de filtragem para todos os provedores registrados com uma instância `ILoggerFactory` usando o método de extensão `WithFilter`. O exemplo abaixo limita os logs de estrutura (categoria começa com "Microsoft" ou "Sistema") a avisos, permitindo que aplicativo registre no nível de depuração.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Se quiser usar a filtragem para impedir que todos os logs de uma determinada categoria sejam gravados, você poderá especificar `LogLevel.None` como o nível de log mínimo para essa categoria. O valor inteiro de `LogLevel.None` é 6, que é maior do que `LogLevel.Critical` (5).

O método de extensão `WithFilter` é fornecido pelo pacote NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter). O método retorna um nova instância `ILoggerFactory`, que filtrará as mensagens de log passadas para todos os provedores de agente registrados com ela. Isso não afeta nenhuma outra instância de `ILoggerFactory`, incluindo a instância de `ILoggerFactory` original.

---

## <a name="log-scopes"></a>Escopos de log

Você pode agrupar um conjunto de operações lógicas dentro de um *escopo* para anexar os mesmos dados a cada log que é criado como parte desse conjunto. Por exemplo, é conveniente que cada log criado como parte do processamento de uma transação inclua a ID da transação.

Um escopo é um tipo `IDisposable` retornado pelo método `ILogger.BeginScope<TState>` e que dura até que seja descartado. Um escopo é usado pelo encapsulamento das chama de agente em um bloco `using`, conforme mostrado aqui:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

O código a seguir habilita os escopos para o provedor de console:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Em *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> A configuração da opção de agente de console `IncludeScopes` é necessária para habilitar o registro em log baseado em escopo. A configuração de `IncludeScopes` usando arquivos de configuração *appsettings* estará disponível com o lançamento do ASP.NET Core 2.1.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Em *Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

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

## <a name="built-in-logging-providers"></a>Provedores de log internos

O ASP.NET Core vem com os seguintes provedores:

* [Console](#console)
* [Depurar](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Serviço de Aplicativo do Azure](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>O provedor de console

O pacote de provedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envia a saída de log para o console. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

As [sobrecargas do AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permitem que você passe um nível de log mínimo, uma função de filtro e um valor booliano que indica se escopos são compatíveis. Outra opção é passar um objeto `IConfiguration`, que pode especificar suporte de escopos e níveis de log. 

Se você estiver considerando o provedor de console para uso em produção, lembre-se de que ele tem um impacto significativo no desempenho.

Quando você cria um novo projeto no Visual Studio, o método `AddConsole` tem essa aparência:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Esse código se refere à seção `Logging` do arquivo *appSettings.json*:

[!code-json[](index/sample//appsettings.json)]

As configurações mostradas limitam os logs de estrutura a avisos, permitindo que o aplicativo para faça registros no nível de depuração, conforme explicado na [Filtragem de log](#log-filtering) seção. Para obter mais informações, consulte [Configuração](xref:fundamentals/configuration/index).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>O provedor Depuração

O pacote de provedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) grava a saída de log usando a classe [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (chamadas de método `Debug.WriteLine`).

No Linux, esse provedor grava logs em */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

As [sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permitem que você passe um nível de log mínimo ou uma função de filtro.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>O provedor EventSource

Para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou superior, o pacote de provedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) pode implementar o rastreamento de eventos. No Windows, ele usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). O provedor é multiplataforma, mas ainda não há ferramentas de coleta e exibição de eventos para Linux ou macOS. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Uma boa maneira de coletar e exibir logs é usar o [utilitário PerfView](https://www.microsoft.com/download/details.aspx?id=28567). Há outras ferramentas para exibir os logs do ETW, mas o PerfView proporciona a melhor experiência para trabalhar com os eventos de ETW emitidos pelo ASP.NET. 

Para configurar o PerfView para coletar eventos registrados por esse provedor, adicione a cadeia de caracteres `*Microsoft-Extensions-Logging` à lista **Provedores Adicionais**. (Não se esqueça do asterisco no início da cadeia de caracteres).

![Outros provedores de Perfview](index/_static/perfview-additional-providers.png)

A captura de eventos no Nano Server demanda algumas configurações adicionais:

* Conecte a comunicação remota do PowerShell ao Nano Server:

  ```powershell
  Enter-PSSession [name]
  ```

* Crie uma sessão de ETW:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Adicione provedores de ETW ao [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ao ASP.NET Core e a outros, conforme necessário. O GUID de provedor do ASP.NET Core é `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Execute o site e realize as ações para as quais você deseja obter informações de rastreamento.

* Interrompa a sessão de rastreamento quando tiver terminado:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

O arquivo resultante *C:\trace.etl* pode ser analisado com PerfView, como em outras edições do Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>O provedor EventLog do Windows

O pacote de provedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envia a saída de log para o Log de Eventos do Windows.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

As [sobrecargas de AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permitem que você passe `EventLogSettings` ou um nível de log mínimo.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>O provedor TraceSource

O pacote de provedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa as bibliotecas e provedores de [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

As [sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permitem que você passe um comutador de fonte e um ouvinte de rastreamento.

Para usar esse provedor, o aplicativo deve ser executado no .NET Framework (em vez do .NET Core). O provedor permite rotear mensagens a uma variedade de [ouvintes](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), como o [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) usado no aplicativo de exemplo.

O exemplo a seguir configura um provedor `TraceSource` que registra mensagens `Warning` e superiores na janela de console.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>O provedor do Serviço de Aplicativo do Azure

O pacote de provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) grava logs em arquivos de texto no sistema de arquivos de um aplicativo do Serviço de Aplicativo do Azure e no [armazenamento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) em uma conta de Armazenamento do Azure. O provedor está disponível somente para aplicativos que se destinam ao ASP.NET Core 1.1.0 ou superior. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se o destino for o .NET Core, você não precisará instalar o pacote de provedor ou chamar explicitamente `AddAzureWebAppDiagnostics`. O provedor fica automaticamente disponível para o aplicativo quando você implanta o aplicativo do Serviço de Aplicativo do Azure.

Se o destino for o .NET Framework, adicione o pacote de provedor ao seu projeto e invoque `AddAzureWebAppDiagnostics`:

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Uma sobrecarga `AddAzureWebAppDiagnostics` permite que você passe [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) com a qual você pode substituir as configurações padrão, como o modelo de saída de registro em log, o nome do blob e o limite do tamanho do arquivo. (O *modelo Output* é um modelo de mensagem que é aplicado a todos os logs, sobre aquele que você fornece ao chamar um método `ILogger`).

---

Ao implantar um aplicativo do Serviço de Aplicativo, seu aplicativo respeita as configurações na seção [Logs de Diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) da página **Serviço de Aplicativo** do Portal do Azure. Ao alterar essas configurações, elas entram em vigor imediatamente, sem exigir que você reinicie o aplicativo ou reimplante o código nele. 

![Configurações de registro em log do Azure](index/_static/azure-logging-settings.png)

O local padrão para arquivos de log é na pasta *D:\\home\\LogFiles\\Application* e o nome de arquivo padrão é *diagnostics-aaaammdd.txt*. O limite padrão de tamanho do arquivo é 10 MB e o número padrão máximo de arquivos mantidos é 2. O nome de blob padrão é *{app-name}{timestamp}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*. Para obter mais informações sobre o comportamento padrão, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

O provedor funciona somente quando o projeto é executado no ambiente do Azure. Ele não tem nenhum efeito quando é executado localmente &mdash; ele não grava em arquivos locais ou no armazenamento de desenvolvimento local para blobs.

## <a name="third-party-logging-providers"></a>Provedores de log de terceiros

Aqui estão algumas estruturas de registros de terceiros que funcionam com o ASP.NET Core:

* [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) – provedor para o serviço Elmah.Io

* [JSNLog](http://jsnlog.com) – registra as exceções de JavaScript e outros eventos do lado do cliente no registro do lado do servidor.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) – provedor para o serviço Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) – provedor para a biblioteca NLog

* [Serilog](https://github.com/serilog/serilog-extensions-logging) – provedor para a biblioteca Serilog

Algumas estruturas de terceiros podem fazer o [log semântico, também conhecido como registro em log estruturado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

O uso de uma estrutura de terceiros é semelhante ao uso de um dos provedores internos: adicione um pacote NuGet ao seu projeto e chame um método de extensão em `ILoggerFactory`. Para obter mais informações, consulte a documentação de cada estrutura.

Você também pode criar seus próprios provedores personalizados para dar suporte a outras estruturas de registros ou a seus próprios requisitos de registro em log.

## <a name="azure-log-streaming"></a>Fluxo de log do Azure

O fluxo de log do Azure permite que você exiba a atividade de log em tempo real: 

* Do servidor de aplicativos 
* Do servidor Web
* De uma solicitação de rastreio com falha 

Para configurar o fluxo de log do Azure: 

* Navegue até a página **Logs de Diagnóstico** da página do portal do seu aplicativo
* Defina o **Log de aplicativo (Sistema de Arquivos)** como ativado. 

![Página de logs de diagnóstico do Portal do Azure](index/_static/azure-diagnostic-logs.png)

Navegue até a página **Fluxo de Log** para exibir as mensagens de aplicativo. Elas são registradas pelo aplicativo por meio da interface `ILogger`. 

![Fluxo de log do aplicativo do Portal do Azure](index/_static/azure-log-streaming.png)


## <a name="see-also"></a>Consulte também

[Registro em log de alto desempenho com LoggerMessage](xref:fundamentals/logging/loggermessage)
