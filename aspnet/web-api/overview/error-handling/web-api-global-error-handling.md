---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Global tratamento de erros em ASP.NET Web API 2 | Microsoft Docs
author: davidmatson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d2bdf04b4da2a099f3a2af100b16682c68f946f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Global tratamento de erros em ASP.NET Web API 2
====================
por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Atualmente, não é possível fácil na API da Web para registrar ou manipular erros globalmente. Algumas exceções sem tratamento podem ser processadas por meio de [filtros de exceção](exception-handling.md), mas há um número de casos que não é possível lidar com os filtros de exceção. Por exemplo:

1. Exceções geradas por construtores de controlador.
2. Exceções geradas por manipuladores de mensagens.
3. Exceções geradas durante o roteamento.
4. Exceções geradas durante a serialização de conteúdo da resposta.

Queremos fornecem uma maneira simple e consistente de log e manipular (onde for possível) essas exceções. 

Há dois casos principais para tratamento de exceções, o caso onde podemos enviar uma resposta de erro e o caso em que tudo o que pode fazer é a exceção de log. Um exemplo para o último caso é quando uma exceção é lançada no meio de streaming de conteúdo da resposta; Nesse caso é muito tarde para enviar uma nova mensagem de resposta, desde que o código de status, cabeçalhos e conteúdo parcial já passaram pela rede, portanto, podemos simplesmente abortar a conexão. Mesmo que a exceção não pode ser manipulada para produzir uma nova mensagem de resposta, oferecemos suporte ainda para registrar a exceção. Em casos onde podemos detectar um erro, podemos pode retornar uma resposta de erro apropriado conforme mostrado no exemplo a seguir:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opções existentes

Além [filtros de exceção](exception-handling.md), [manipuladores de mensagens](../advanced/http-message-handlers.md) pode ser usado atualmente para observar todas as respostas de nível 500, mas atuar nessas respostas é difícil, pois eles não têm contexto sobre o erro original. Manipuladores de mensagens também tem as mesmas limitações que os filtros de exceção sobre os casos em que eles possam lidar com alguns. Enquanto a API da Web tem uma infraestrutura de rastreamento que captura as condições de erro a infra-estrutura de rastreamento é para fins de diagnóstico e não é projetada ou adequada para a execução em ambientes de produção. Log e tratamento de exceção global deve ser serviços que podem ser executado durante a produção e ser conectados a soluções de monitoramento existentes (por exemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Visão geral da solução

 Fornecemos dois novos serviços substituível pelo usuário, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, fazer logon e lidar com exceções sem tratamento. Os serviços são muito semelhantes, com duas diferenças principais:

1. Há suporte para registrar vários agentes de exceção, mas apenas um único manipulador de exceções.
2. Agentes de exceção sempre seja chamados, mesmo se estiver prestes a anular a conexão. Manipuladores de exceção obtenham chamados apenas quando estamos ainda poderá escolher qual mensagem de resposta para enviar.

Ambos os serviços fornecem acesso a um contexto de exceção que contém informações relevantes do ponto onde a exceção foi detectada, particularmente o [HttpRequestMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessage(v=vs.110).aspx), o [HttpRequestContext](https://msdn.microsoft.com/en-us/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), o gerada exceção e a origem da exceção (detalhes abaixo).

### <a name="design-principles"></a>Princípios de design

1. **Não há alterações significativas** porque essa funcionalidade está sendo adicionada em uma versão secundária, uma restrição importante, afetando a solução é que não haver nenhum alterações de quebra, para o tipo de contratos ou comportamento. Essa restrição excluído alguma limpeza, gostaríamos de ter feito em termos de blocos catch existentes exceções em 500 respostas a ativação. Essa limpeza adicional é algo que talvez seja considerada para uma versão principal subsequente. Se isso é importante para você, vote em [voz do usuário ASP.NET Web API](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Manter a consistência com a API da Web constrói** pipeline de filtro da API da Web é uma ótima maneira de lidar com resolvem preocupações com a flexibilidade de aplicar a lógica em um escopo específico de ação, controlador específico ou global. Filtros, inclusive filtros de exceção, sempre têm contextos de ação e controlador, mesmo quando registrado no escopo global. Que contrato faz sentido para filtros, mas isso significa que filtros de exceção, os mesmo globalmente no escopo, não são uma boa opção para alguns casos, como exceções de manipuladores de mensagens de manipulação onde nenhum contexto de ação ou controlador de exceção não existe. Se queremos usar o escopo flexível proporcionados pelos filtros para manipulação de exceção, ainda precisamos filtros de exceção. Mas, se é necessário manipular exceção fora de um contexto de controlador, também precisamos de uma construção separada para tratamento de erros completa global (algo sem os controlador contexto e ação contexto restrições).

### <a name="when-to-use"></a>Quando usar

- Agentes de exceção são a solução para ver todos os exceção não tratada detectada pela API da Web.
- Manipuladores de exceção são as soluções para personalização de todas as respostas possíveis a exceções sem tratamento capturadas pela API da Web.
- Filtros de exceção são a solução mais fácil para processar as exceções sem tratamento de subconjunto relacionadas a um controlador ou ação específica.

### <a name="service-details"></a>Detalhes do serviço

 As interfaces de serviço de agente de log e o manipulador de exceção são métodos assíncronos simples colocar os respectivos contextos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Nós também fornecemos classes base para as duas interfaces. Substituindo os métodos de núcleo (sincronização ou assíncrona) é tudo o que é necessário fazer logon ou tratar recomendada vezes. Para fazer logon, o `ExceptionLogger` classe base garantirá que o método de registro em log de núcleo só é chamado uma vez para cada exceção (mesmo que mais tarde se propaga adicional até a pilha de chamadas e fique novamente). O `ExceptionHandler` classe base chamará o núcleo de método de manipulação apenas para blocos de captura exceções no topo da pilha de chamadas, ignorando herdado aninhado. (Versões simplificadas dessas classes base são no Apêndice a seguir). Ambos `IExceptionLogger` e `IExceptionHandler` receber informações sobre a exceção por meio de um `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando o framework chama um manipulador de exceção ou de um agente de exceção, ele sempre fornecerá uma `Exception` e um `Request`. Exceto para testes de unidade, ele também fornecerá um `RequestContext`. Ele raramente fornecerá uma `ControllerContext` e `ActionContext` (somente ao chamar do bloco catch para filtros de exceção). Muito raramente fornecerá um `Response`(somente em determinados casos IIS quando no meio ao tentar gravar a resposta). Observe que, como algumas dessas propriedades podem ser `null` depende do consumidor para verificar se há `null` antes de acessar os membros da classe de exceção.`CatchBlock` é uma cadeia de caracteres que indica o bloco catch viu a exceção. As cadeias de caracteres do bloco catch são da seguinte maneira:

- Servidor HTTP (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (processamento do ApiController do pipeline de filtro de exceção em ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (para o buffer de saída)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (para streaming de saída)
- Host da Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (para o buffer de saída)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (para streaming de saída)
    - HttpControllerHandler.WriteErrorResponseContentAsync (para falhas na recuperação de erro em modo de buffer de saída)

A lista de cadeias de caracteres de bloco catch também está disponível por meio de propriedades somente leitura e estático. (A cadeia de caracteres do bloco de catch principais estão no ExceptionCatchBlocks estático; o restante aparecem em uma classe estática cada host OWIN e da web).`IsTopLevelCatchBlock` é útil para seguir o padrão recomendado de tratamento de exceções somente na parte superior da pilha de chamadas. Em vez de ativar exceções em 500 respostas em qualquer lugar, que ocorre um bloco catch aninhado, um manipulador de exceção pode deixar exceções propagar até que eles devem ser vistos pelo host.

Além de `ExceptionContext`, um agente de log é uma informação mais via completa `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

A segunda propriedade, `CanBeHandled`, permite que um agente de log identificar uma exceção que não pode ser manipulada. Quando a conexão está prestes a ser anulada e nenhuma nova mensagem de resposta pode ser enviada, os agentes de log serão chamados mas o manipulador ***não*** ser chamado, e os agentes de log podem identificar este cenário dessa propriedade.

Em adicional para o `ExceptionContext`, um manipulador obtém mais uma propriedade pode ser definida em completa `ExceptionHandlerContext` a manipulação da exceção:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Um manipulador de exceção indica que tratou uma exceção ao definir o `Result` propriedade como resultado de uma ação (por exemplo, um [ExceptionResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ou um resultado personalizado). Se o `Result` propriedade for nula, a exceção é sem tratamento e a exceção original será lançada novamente.

Para exceções na parte superior da pilha de chamadas, demos uma etapa extra para garantir que a resposta é apropriada para chamadores de API. Se a exceção for propagada até o host, o chamador deve ver a tela amarela de morte ou algum outro host fornecido resposta que normalmente é HTML e geralmente não são uma resposta de erro de API apropriada. Nesses casos, inicia o resultado não nulo e somente se um manipulador de exceções define explicitamente de volta para `null` (sem tratamento) serão a exceção propagadas para o host. Configuração `Result` para `null` nesses casos pode ser útil para dois cenários:

1. OWIN hospedado API da Web com a exceção personalizada tratamento middleware registrado antes/externa API da Web.
2. Depuração local por meio de um navegador, em que a tela de amarelo morte é, na verdade, uma resposta útil para uma exceção sem tratamento.

Para agentes de exceção e manipuladores de exceção, nós não fazer nada para recuperar se o agente de log ou o próprio Gerenciador de lançar uma exceção. (Além de permitir que a exceção se propague, deixe comentários na parte inferior desta página, se você tiver uma abordagem melhor.) O contrato para agentes de exceção e manipuladores é que eles devem não permitem exceções propagar até os chamadores; Caso contrário, a exceção apenas propagarão, frequentemente totalmente para o host, resultando em um erro HTML (como o ASP. Tela de amarelo do NET) que está sendo enviada de volta ao cliente (que normalmente não é a opção preferencial para chamadores de API que espera JSON ou XML).

## <a name="examples"></a>Exemplos

### <a name="tracing-exception-logger"></a>Agente de exceção de rastreamento

O agente de exceção abaixo enviar dados de exceção para fontes de rastreamento configurados (incluindo a janela de saída de depuração no Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Manipulador de exceção de mensagem de erro personalizada

O seguinte abaixo produz uma resposta de erro personalizado para clientes, incluindo um endereço de email para entrar em contato com o suporte.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registro de filtros de exceção

Se você usar o modelo de projeto "Aplicativo de Web do ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração de API da Web dentro do `WebApiConfig` classe, o *aplicativo/i_niciar* pasta:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apêndice: Detalhes da classe Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
