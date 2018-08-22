---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Global tratamento de erro no ASP.NET Web API 2 | Microsoft Docs
author: davidmatson
description: ''
ms.author: riande
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: a52c2a1589327421b7f498ff551145676c80e3e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834091"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Global tratamento de erros em API Web ASP.NET 2
====================
por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Hoje, há nenhuma maneira fácil na API da Web para fazer logon ou tratar erros globalmente. Algumas exceções não tratadas podem ser processadas por meio [filtros de exceção](exception-handling.md), mas há um número de casos que não é possível lidar com os filtros de exceção. Por exemplo:

1. Exceções geradas por construtores de controlador.
2. Exceções geradas por manipuladores de mensagens.
3. Exceções geradas durante o roteamento.
4. Exceções geradas durante a serialização de conteúdo da resposta.

Queremos fornecer uma maneira simples e consistente para fazer logon e tratar (quando possível) essas exceções. 

Há dois casos principais para tratamento de exceções, caso em que somos capazes de enviar uma resposta de erro e o caso em que tudo o que pode fazer é a exceção do log. Um exemplo no segundo caso é quando uma exceção é lançada no meio de streaming de conteúdo da resposta; Nesse caso, é tarde demais para enviar uma nova mensagem de resposta, pois o código de status, cabeçalhos e conteúdo parcial já passaram pela rede, portanto, podemos simplesmente abortar a conexão. Mesmo que a exceção não pode ser manipulada para produzir uma nova mensagem de resposta, que ainda damos suporte registrar a exceção. Em casos em que podemos detectar um erro, podemos pode retornar uma resposta de erro apropriado conforme mostrado a seguir:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opções existentes

Além [filtros de exceção](exception-handling.md), [manipuladores de mensagem](../advanced/http-message-handlers.md) pode ser usada hoje para observar todas as respostas de nível 500, mas agindo nessas respostas é difícil, pois eles não têm um contexto sobre o erro original. Manipuladores de mensagens também tem algumas das mesmas limitações sobre os casos em que eles possam lidar com os filtros de exceção. Enquanto a API da Web tem a infraestrutura de rastreamento que captura as condições de erro a infra-estrutura de rastreamento é para fins de diagnóstico e não é desenvolvida ou adequada para executar em ambientes de produção. Log e tratamento de exceção global deve ser a serviços que podem ser executado durante a produção e podem ser conectados em soluções de monitoramento existentes (por exemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Visão geral da solução

 Nós fornecemos dois novos serviços substituível pelo usuário, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, fazer logon e tratam exceções sem tratamento. Os serviços são muito semelhantes, com duas diferenças principais:

1. Há suporte para registrar vários agentes de exceção, mas apenas um único manipulador de exceções.
2. Agentes de exceção sempre seja chamados, mesmo que o que estamos prestes a anular a conexão. Manipuladores de exceção apenas ser chamados quando estamos ainda pode escolher para enviar a mensagem de resposta.

Ambos os serviços fornecem acesso a um contexto de exceção que contém informações relevantes do ponto em que a exceção foi detectada, particularmente o [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), o [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), o gerada de exceção e a origem da exceção (detalhes abaixo).

### <a name="design-principles"></a>Princípios de design

1. **Nenhuma alteração significativa** porque essa funcionalidade está sendo adicionada em uma versão secundária, uma restrição importante que afetam a solução é que não haver nenhuma alteração significativa, qualquer um para o tipo de contratos ou comportamento. Essa restrição excluído uma limpeza, gostaríamos de ter feito em termos de blocos catch existentes, ativar exceções em 500 respostas. Essa limpeza adicional é algo que podemos pode considerar para uma versão principal subsequente. Se isso for importante para você, vote nele em [voz do usuário de API Web ASP.NET](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Manter a consistência com a API Web constrói** pipeline de filtro da API Web é uma ótima maneira de lidar com questões abrangentes com a flexibilidade de aplicar a lógica em um escopo de ação específicos, específica ao controlador ou global. Filtros, incluindo filtros de exceção, sempre têm contextos de ação e controlador, mesmo quando registrado no escopo global. Que contrato faz sentido para os filtros, mas isso significa que os filtros de exceção, aqueles com escopo definido globalmente até mesmo, não são uma boa opção para alguns casos, como exceções de manipuladores de mensagens, de manipulação onde nenhum contexto de controlador ou ação de exceção existe. Se desejamos usar o escopo flexível proporcionada pelo filtros para manipulação de exceção, ainda precisamos filtros de exceção. Mas se precisamos lidar com a exceção fora de um contexto de controlador, também precisamos de uma construção separada global completa para tratamento de erros (algo sem os controlador contexto e a ação de contexto restrições).

### <a name="when-to-use"></a>Quando usar

- Agentes de exceção são a solução para ver todos os exceção não tratada detectada pela API da Web.
- Manipuladores de exceção são a solução para personalizar todas as respostas possíveis a exceções sem tratamento capturadas pela API da Web.
- Filtros de exceção são a solução mais fácil para processar as exceções sem tratamento do subconjunto relacionadas a uma ação específica ou controlador.

### <a name="service-details"></a>Detalhes do serviço

 As interfaces de serviço de agente e o manipulador de exceção são métodos de async simples levando os respectivos contextos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Nós também fornecemos classes base para essas duas interfaces. Substituindo os métodos do core (síncrona ou assíncrona) é tudo o que é necessário para fazer logon ou manipular em recomendado vezes. Para fazer logon, o `ExceptionLogger` classe base garantirá que o método de registro em log core só é chamado uma vez para cada exceção (mesmo que ele mais tarde se propaga ainda mais para cima a pilha de chamadas e é capturada novamente). O `ExceptionHandler` classe base chamará o principal método de manipulação, apenas para os blocos catch de exceções na parte superior da pilha de chamadas, ignorando herdado aninhado. (Versões simplificadas dessas classes base são no Apêndice a seguir.) Ambos `IExceptionLogger` e `IExceptionHandler` receber informações sobre a exceção por meio de um `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando o framework chama um manipulador de exceção ou de um agente de exceção, ele sempre fornecerá uma `Exception` e um `Request`. Exceto para testes de unidade, ele sempre também fornecerá um `RequestContext`. Ele raramente fornecerá uma `ControllerContext` e `ActionContext` (somente ao chamar o bloco de captura para filtros de exceção). Muito raramente fornecerá um `Response`(somente em determinados casos IIS quando no meio de tentativa de gravar a resposta). Observe que, como algumas dessas propriedades podem ser `null` depende do consumidor para verificar se há `null` antes de acessar membros de classe de exceção.`CatchBlock` é uma cadeia de caracteres que indica qual bloco catch viu a exceção. As cadeias de caracteres do bloco catch são da seguinte maneira:

- Servidor HTTP (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (processamento do ApiController do pipeline de filtros de exceção em ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (para buffer de saída)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (para streaming de saída)
- Host da Web:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (para buffer de saída)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (para streaming de saída)
    - HttpControllerHandler.WriteErrorResponseContentAsync (para falhas na recuperação de erro em modo de saída em buffer)

A lista de cadeias de caracteres de bloco catch também está disponível por meio de propriedades somente leitura estático. (A cadeia de caracteres do bloco de catch core são sobre o ExceptionCatchBlocks estático; o restante são exibidos em uma classe estática cada host OWIN e da web).`IsTopLevelCatchBlock` é útil para seguir o padrão recomendado de tratamento de exceções apenas na parte superior da pilha de chamadas. Em vez de transformar exceções em 500 respostas em qualquer lugar em que um bloco catch aninhado ocorre, um manipulador de exceção pode permitir que exceções se propagar até que elas estejam prestes a ser visto pelo host.

Além de `ExceptionContext`, um agente obtém mais uma parte das informações por meio de todo o `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

A segunda propriedade, `CanBeHandled`, permite que um agente de log identificar uma exceção que não pode ser manipulada. Quando a conexão está prestes a ser anulada e nenhuma nova mensagem de resposta pode ser enviada, os agentes de log serão chamados, mas o manipulador será ***não*** ser chamado, e os agentes de log podem identificar este cenário dessa propriedade.

Em adicional para o `ExceptionContext`, um manipulador obtém mais de uma propriedade pode ser definida em todo o `ExceptionHandlerContext` para tratar a exceção:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Um manipulador de exceção indica que tratou uma exceção, definindo o `Result` propriedade para um resultado de ação (por exemplo, um [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ou um resultado personalizado). Se o `Result` propriedade for nula, a exceção é sem tratamento e a exceção original será relançada.

Para exceções na parte superior da pilha de chamadas, tivemos uma etapa extra para garantir que a resposta é apropriada para chamadores de API. Se a exceção for propagada até o host, o chamador vê a tela amarela morte ou algum outro host fornecido resposta que normalmente é HTML e geralmente não uma resposta de erro de API apropriada. Nesses casos, inicia o resultado não nulo, e somente se um manipulador de exceção personalizada define explicitamente de volta para `null` (sem tratamento) será a exceção propagar para o host. Definindo `Result` para `null` nesses casos, pode ser útil para dois cenários:

1. Web API hospedada no OWIN com registrou a API da Web antes de/fora de middleware de tratamento de exceção personalizada.
2. Depuração local por meio de um navegador, em que a tela amarela morte é, na verdade, uma resposta úteis para uma exceção sem tratamento.

Para agentes de exceção e manipuladores de exceção, não fazemos nada a recuperar se o agente de log ou o próprio manipulador gera uma exceção. (Diferente de deixar a exceção propagar, deixar comentários na parte inferior desta página, se você tem uma abordagem melhor.) O contrato para agentes de exceção e manipuladores é que elas jamais devesse permitir que exceções se propagar até seus chamadores; Caso contrário, a exceção apenas propagarão, muitas vezes até o host, resultando em um erro HTML (como o ASP. Tela de amarelo da rede) que está sendo enviada de volta ao cliente (que normalmente não é a opção preferencial para os chamadores de API que esperam o JSON ou XML).

## <a name="examples"></a>Exemplos

### <a name="tracing-exception-logger"></a>Agente de exceção de rastreamento

O agente de exceção abaixo enviar dados de exceção para origens de rastreamento configuradas (incluindo a janela de saída de depuração no Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Manipulador de exceção de mensagem de erro personalizada

O seguinte abaixo produz uma resposta de erro personalizado para clientes, incluindo um endereço de email para entrar em contato com o suporte.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrando os filtros de exceção

Se você usar o modelo de projeto "Aplicativo de Web do ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração de API da Web dentro do `WebApiConfig` classe, o *iniciar* pasta:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apêndice: Detalhes da classe Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
