---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: "Usando métodos assíncronos no ASP.NET MVC 4 | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo da Web do ASP.NET MVC assíncrono usando o Visual Studio Express 2012 para Web, que é um var livre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b4280c6ab1b6d8d2ceaa7cef14fce94ab8c6df53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Usando métodos assíncronos no ASP.NET MVC 4
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo Web do ASP.NET MVC assíncrona usando [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que é uma versão gratuita do Microsoft Visual Studio. Você também pode usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).

> Um exemplo é fornecido para este tutorial no github [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


O ASP.NET MVC 4 [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) classe em combinação [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permite que você escreva métodos de ação assíncrono que retorna um objeto do tipo [tarefa&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). O .NET Framework 4 introduziu um conceito de programação assíncrono conhecido como um [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e dá suporte ao ASP.NET MVC 4 [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Tarefas são representadas pelo **tarefa** tipo e tipos relacionados no [Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) namespace. O .NET Framework 4.5 amplia esse suporte assíncrono com o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave que tornam o trabalho com [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos muito menos complexos do que o anterior métodos assíncronos. O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave é a abreviação sintática para indicar que um trecho de código deve aguardar assincronamente em alguma outra parte do código. O [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave representa uma dica que você pode usar para marcar métodos como métodos assíncronos baseado em tarefa. A combinação de **await**, **async**e o **tarefa** objeto torna muito mais fácil para escrever código assíncrono no .NET 4.5. O novo modelo para métodos assíncronos é chamado de *padrão assíncrono baseado em tarefa* (**toque**). Este tutorial presume que você tenha alguma familiaridade com programação assíncrona usando [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace.

Para obter mais informações sobre o uso de [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte as seguintes referências.

- [White paper: Assincronia no .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Perguntas Frequentes de Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programação assíncrona do Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Como as solicitações são processadas pelo Pool de Thread

No servidor web, o .NET Framework mantém um pool de threads que são usados para atender a solicitações ASP.NET. Quando uma solicitação chega, um thread do pool é enviado para processar essa solicitação. Se a solicitação é processada de forma síncrona, o thread que processa a solicitação está ocupado durante a solicitação está sendo processado e que o thread não pode atender outra solicitação.   
  
Isso não pode ser um problema, porque o pool de threads pode se tornar grande o suficiente para acomodar o número de threads ocupado. No entanto, o número de threads no pool de threads é limitado (o padrão máximo para o .NET 4.5 é 5.000). Em aplicativos grandes com simultaneidade alta de solicitações de execução longa, todos os threads disponíveis podem estar ocupados. Essa condição é conhecida como escassez de threads. Quando essa condição for atingida, o servidor web enfileira as solicitações. Se a fila de solicitações ficar cheio, o servidor web rejeita solicitações com um status de HTTP 503 (servidor muito ocupado). O pool de threads do CLR tem limitações na nova injeções de thread. Se a simultaneidade é intermitente (isto é, seu site da web pode repentinamente obter um grande número de solicitações) e todos os threads de solicitação disponíveis estão ocupados devido a chamadas de back-end com alta latência, a taxa de injeção de thread limitado pode tornar seu aplicativo mal responder. Além disso, cada novo thread adicionado ao pool de threads tem sobrecarga (por exemplo, 1 MB de memória de pilha). Um aplicativo web usando métodos síncronos a chamadas de alta latência de serviço em que o pool de threads aumenta para o .NET 4.5 padrão máximo de 5, 000 threads consumiria aproximadamente 5 GB mais memória do que um aplicativo capaz de iguais as solicitações de serviço usando métodos assíncronos e apenas 50 threads. Quando você estiver fazendo o trabalho assíncrona, você nem sempre está usando um thread. Por exemplo, quando você faz uma solicitação de serviço da web assíncrono, ASP.NET não usará a threads entre o **async** chamada de método e o **await**. Usar o pool de threads para solicitações de serviço com alta latência, isso pode levar a um espaço de memória grandes e a baixa utilização do hardware do servidor.

## <a name="processing-asynchronous-requests"></a>Processamento de solicitações assíncronas

Em aplicativos web que vê um grande número de solicitações simultâneas na inicialização ou tem uma carga intermitente (onde simultaneidade aumenta, de repente,), fazer essas chamadas de serviço web assíncrona aumentará a capacidade de resposta do seu aplicativo. Uma solicitação assíncrona leva a mesma quantidade de tempo para processar uma solicitação síncrona. Por exemplo, se uma solicitação de um serviço web chamar que requer dois segundos ser concluída, a solicitação usa dois segundos se ela é realizada de forma síncrona ou assíncrona. No entanto, durante uma chamada assíncrona, um thread não é bloqueado de responder às outras solicitações enquanto aguarda a primeira solicitação ser concluída. Portanto, solicitações assíncronas evitar o crescimento de pool de enfileiramento de mensagens e thread de solicitação quando há muitas solicitações simultâneas que invocar operações de execução longa.

## <a id="ChoosingSyncVasync"></a>Escolher os métodos de ação síncrono ou assíncrono

Esta seção lista as diretrizes sobre quando usar métodos de ação síncrono ou assíncrono. Essas são apenas diretrizes; Examine cada aplicativo individualmente para determinar se os métodos assíncronos ajudam com o desempenho.

Em geral, use os métodos síncronos para as seguintes condições:

- As operações são simples ou curta execução.
- Simplicidade é mais importante do que a eficiência.
- As operações são principalmente as operações da CPU, em vez de operações que envolvem a sobrecarga de rede ou de disco abrangentes. Usando métodos de ação assíncrona em operações associadas à CPU não fornece nenhuma benefícios e resulta em mais sobrecarga.

 Em geral, use os métodos assíncronos para as seguintes condições:

- Chamada de serviços que podem ser consumidos por meio de métodos assíncronos, e você estiver usando o .NET 4.5 ou posterior.
- As operações são vinculadas à rede ou I/O vinculados em vez de limite de CPU.
- Paralelismo é mais importante do que a simplicidade do código.
- Você deseja fornecer um mecanismo que permite aos usuários cancelar uma solicitação de longa execução.
- Quando o benefício da comutação threads out pondera o custo da alternância de contexto. Em geral, você deve fazer um método assíncrono se o método síncrono aguarda o thread de solicitação do ASP.NET ao não fazer nenhum trabalho. Fazendo a chamada assíncrona, o thread de solicitação do ASP.NET não está paralisado não executando nenhum trabalho enquanto aguarda a solicitação de serviço da web concluir.
- O teste mostra que o bloqueio de operações é um afunilamento no desempenho do site e que o IIS pode atender mais solicitações usando métodos assíncronos para essas chamadas de bloqueio.

 O exemplo disponível para download mostra como usar métodos de ação assíncrono com eficiência. O exemplo fornecido foi projetado para fornecer uma demonstração simple de programação assíncrona no ASP.NET MVC 4 usando o .NET 4.5. O exemplo não pretende ser uma arquitetura de referência para programação assíncrona no ASP.NET MVC. O programa de exemplo chama [ASP.NET Web API](../../../web-api/index.md) métodos que por sua vez chamam [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular chamadas de serviço web de longa execução. A maioria dos aplicativos de produção não mostrará esses benefícios óbvios ao uso de métodos de ação assíncrono.   
  
Alguns aplicativos exigem que todos os métodos de ação a ser assíncrona. Geralmente, a conversão de alguns métodos de ação síncrono para métodos assíncronos fornece o aumento de eficiência melhor para a quantidade de trabalho necessário.

## <a id="SampleApp"></a>O aplicativo de exemplo

Você pode baixar o aplicativo de exemplo do [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) no [GitHub](https://github.com/) site. O repositório consiste em três projetos:

- *Mvc4Async*: projeto o ASP.NET MVC 4 que contém o código usado neste tutorial. Ele faz chamadas de API da Web para o **WebAPIpgw** service.
- *WebAPIpgw*: projeto de API da Web do ASP.NET MVC 4 que implementa o `Products, Gizmos and Widgets` controladores. Ele fornece os dados para o *WebAppAsync* projeto e o *Mvc4Async* projeto.
- *WebAppAsync*: projeto o ASP.NET Web Forms usado em outro tutorial.

## <a id="GizmosSynch"></a>O método de ação síncrono largue as

 O código a seguir mostra o `Gizmos` método de ação síncrono que é usado para exibir uma lista de largue as. (Neste artigo, um gizmo é um dispositivo mecânico fictício.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

O código a seguir mostra o `GetGizmos` método do serviço gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

O `GizmoService GetGizmos` método passa um URI para um serviço ASP.NET Web API HTTP que retorna uma lista de dados largue as. O *WebAPIpgw* projeto contém a implementação da API da Web `gizmos, widget` e `product` controladores.  
A imagem a seguir mostra a exibição de largue as do projeto de exemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Criar um método de ação largue as assíncrona

O exemplo usa o novo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavras-chave (disponível no .NET 4.5 e o Visual Studio 2012) para permitir que o compilador serão responsáveis por manter as transformações complicadas necessárias para programação assíncrona. O compilador permite que você escreva o código usando que fluxo de controle síncrono do # constrói e o compilador aplica automaticamente as transformações necessárias para usar retornos de chamada para evitar o bloqueio de threads.

O código a seguir mostra o `Gizmos` método síncrono e o `GizmosAsync` método assíncrono. Se o seu navegador suporta o [HTML 5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), você verá as alterações no `GizmosAsync` no realce amarela.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 As seguintes alterações foram aplicadas para permitir que o `GizmosAsync` seja assíncrona.

- O método está marcado com o [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, que informa ao compilador para gerar retornos de chamada para partes do corpo e criar automaticamente um `Task<ActionResult>` que é retornado.
- &quot;Assíncrono&quot; foi acrescentado ao nome do método. Anexar "Async" não é necessário, mas é a convenção ao escrever métodos assíncronos.
- O tipo de retorno foi alterado de `ActionResult` para `Task<ActionResult>`. O tipo de retorno `Task<ActionResult>` representa um trabalho contínuo e fornece chamadores do método com um identificador por meio do qual a aguardar pela conclusão da operação assíncrona. Nesse caso, o chamador é o serviço da web. `Task<ActionResult>`representa em andamento funciona com o resultado`ActionResult.`
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada a chamada de serviço web.
- A API do serviço da web assíncrono foi chamada (`GetGizmosAsync`).

Dentro do `GetGizmosAsync` outro método assíncrono, o corpo do método `GetGizmosAsync` é chamado. `GetGizmosAsync`retorna imediatamente um `Task<List<Gizmo>>` que eventualmente será concluída quando os dados estão disponíveis. Porque você não quiser fazer mais nada até que você tenha os dados de gizmo, o código aguarda a tarefa (usando o **await** palavra-chave). Você pode usar o **await** palavra-chave somente em métodos anotados com o **async** palavra-chave.

O **await** palavra-chave não bloqueia o thread até que a tarefa seja concluída. Ele se inscreve o restante do método como um retorno de chamada da tarefa e retorna imediatamente. Quando a tarefa esperada eventualmente for concluído, ele chamar o retorno de chamada e, assim, retomar a execução do direito de método em que parou. Para obter mais informações sobre como usar o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte o [async referências](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

O código a seguir mostra o `GetGizmos` e `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 As alterações assíncronas são semelhantes às feitas a **GizmosAsync** acima. 

- A assinatura do método foi anotada com a [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, o tipo de retorno foi alterado para `Task<List<Gizmo>>`, e *Async* foi acrescentado ao nome do método.
- O assíncrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe é usada em vez do [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada para o [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) métodos assíncronos.

A imagem a seguir mostra a exibição de gizmo assíncrona.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

A apresentação de navegadores dos dados largue as é idêntica ao modo de exibição criado pela chamada síncrona. A única diferença é que a versão assíncrona pode ser um desempenho melhor com cargas pesadas.

## <a id="Parallel"></a>Executar várias operações em paralelo

Métodos de ação assíncrono tem uma vantagem significativa sobre métodos síncronos quando uma ação deve executar várias operações independentes. No exemplo fornecido, o método síncrono `PWG`(para produtos, Widgets e largue as) exibe os resultados de três chamadas de serviço da web para obter uma lista de produtos, widgets e largue as. O [ASP.NET Web API](../../../web-api/index.md) projeto que fornece esses serviços usa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular a latência de rede lenta a chamadas. Quando o atraso é definido como 500 milissegundos, assíncronos `PWGasync` método usa um pouco mais de 500 milissegundos para concluir durante síncronos `PWG` versão assume 1.500 milésimos de segundo. Síncronos `PWG` método é mostrado no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

O assíncrona `PWGasync` método é mostrado no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

A imagem a seguir mostra a exibição retornada do **PWGasync** método.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Usando um Token de cancelamento

Métodos de ação assíncrono retornando `Task<ActionResult>`são anulável, que é que eles terão uma [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parâmetro quando é fornecida com o [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) atributo. O código a seguir mostra o `GizmosCancelAsync` método com um tempo limite de 150 milissegundos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

O código a seguir mostra a sobrecarga de GetGizmosAsync, que usa um [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parâmetro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

O aplicativo de exemplo fornecido, selecionando o *demonstração de Token de cancelamento* link chamadas a `GizmosCancelAsync` método e demonstra o cancelamento da chamada assíncrona.

## <a id="ServerConfig"></a>Configuração do servidor para o serviço Web de alta simultaneidade/alta latência chamadas

Para obter os benefícios de um aplicativo web assíncrona, você precisará fazer algumas alterações na configuração de servidor padrão. Tenha em mente ao configurar e testar seu aplicativo web assíncrona de estresse.

- Windows 7, Windows Vista e todos os sistemas de operacionais de cliente Windows ter um máximo de 10 solicitações simultâneas. Você precisará de um sistema operacional de servidor do Windows para ver os benefícios dos métodos assíncronos sob alta carga.
- Registre o .NET 4.5 com o IIS em um prompt de comando elevado:  
 %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
 Consulte [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Talvez seja necessário aumentar o [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite da fila do valor padrão de 1.000 a 5.000. Se a configuração é muito baixa, você poderá ver [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rejeitar solicitações com um status de HTTP 503. Para alterar o limite de fila de HTTP. sys:

    - Abra o Gerenciador do IIS e navegue até o painel de Pools de aplicativos.
    - Clique com o botão direito no pool de aplicativos de destino e selecione **configurações avançadas**.  
        ![advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - No **configurações avançadas** caixa de diálogo Alterar *comprimento da fila de* mais de 1.000 a 5.000.  
        ![Comprimento da fila](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
 Observe que nas imagens acima, o .NET framework está listado como v 4.0, mesmo que o pool de aplicativos usando o .NET 4.5. Para entender essa discrepância, consulte o seguinte:

    - [Controle de versão do .NET e o .NET 4.5 Multi-Targeting - é uma atualização in-loco para o .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Como configurar um aplicativo do IIS ou o AppPool usar ASP.NET 3.5 em vez de 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versões e dependências do .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Se seu aplicativo estiver usando serviços web ou System.NET para se comunicar com um back-end a via HTTP, você pode precisar aumentar a [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para aplicativos ASP.NET, isso é limitado pelo recurso de configuração automática para 12 vezes o número de CPUs. Isso significa que em um processo quad, você pode ter no máximo 12 \* 4 = 48 conexões simultâneas com um ponto de extremidade do IP. Porque isso está vinculado ao [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), a maneira mais fácil para aumentar `maxconnection` em um ASP.NET aplicativo é definir [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programaticamente no o de `Application_Start` método o *global. asax* arquivo. Consulte o exemplo de download para obter um exemplo.
- No .NET 4.5, o padrão de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) deve ser suficiente.
