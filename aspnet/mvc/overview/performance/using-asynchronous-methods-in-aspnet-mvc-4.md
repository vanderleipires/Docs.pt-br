---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Usar métodos assíncronos no ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC assíncrono usando o Visual Studio Express 2012 para Web, que é um ve gratuito...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5469ac18f55001b441def5b547b7f1836ee9d052
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833049"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Usando métodos assíncronos no ASP.NET MVC 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial lhe ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC assíncrona usando [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que é uma versão gratuita do Microsoft Visual Studio. Você também pode usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Um exemplo completo é fornecido para este tutorial no github  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


O ASP.NET MVC 4 [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) classe em combinação [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) lhe permite escrever métodos de ação assíncrono que retornam um objeto do tipo [tarefa&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). O .NET Framework 4 introduziu um conceito de programação assíncrono, conhecido como um [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e dá suporte ao ASP.NET MVC 4 [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). As tarefas são representadas pela **tarefa** tipo e tipos relacionados na [Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) namespace. O .NET Framework 4.5 se baseia nesse suporte assíncrono com o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave que tornam o trabalho com [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos muito menos complexos do que a anterior métodos assíncronos. O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave é o tipo de taquigrafia sintático para indicar que um trecho de código deve aguardar de forma assíncrona em alguma outra parte do código. O [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave representa uma dica de que você pode usar para marcar métodos como métodos assíncronos baseados em tarefa. A combinação de **await**, **async**e o **tarefa** objeto torna muito mais fácil de escrever código assíncrono no .NET 4.5. O novo modelo para métodos assíncronos é chamado de *padrão assíncrono baseado em tarefa* (**toque**). Este tutorial presume que você tem alguma familiaridade com o uso de programação assíncrona [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace.

Para obter mais informações sobre como a usar [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte as seguintes referências.

- [White paper: Assincronia no .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Perguntas frequentes sobre Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programação assíncrona do Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Como as solicitações são processadas pelo Pool de segmentos

No servidor web, o .NET Framework mantém um pool de threads que são usados para atender a solicitações do ASP.NET. Quando uma solicitação chega, um thread do pool é despachado para processar essa solicitação. Se a solicitação é processada de forma síncrona, o thread que processa a solicitação está ocupado enquanto a solicitação está sendo processado, e que o thread não pode atender outra solicitação.   
  
Isso não pode ser um problema, porque o pool de threads pode se tornar grande o suficiente para acomodar vários threads ocupados. No entanto, o número de threads no pool de threads é limitado (o padrão máximo para o .NET 4.5 é 5.000). Em grandes aplicativos com alta simultaneidade de solicitações de longa execução, todos os threads disponíveis podem estar ocupados. Essa condição é conhecida como escassez de threads. Quando essa condição for atingida, o servidor web enfileira as solicitações. Se a fila de solicitações ficar cheio, o servidor web rejeita solicitações com um status HTTP 503 (servidor muito ocupado). O pool de threads do CLR tem limitações na nova injeções de thread. Se a simultaneidade for intermitente (ou seja, seu site da web pode, de repente, obter um grande número de solicitações) e todos os threads de solicitação disponíveis são ocupados devido a chamadas de back-end com alta latência, a taxa de injeção limitada de thread pode tornar seu aplicativo responder de forma extremamente ineficiente. Além disso, cada novo thread adicionado ao pool de threads tem sobrecarga (por exemplo, 1 MB de memória de pilha). Um aplicativo web usando os métodos síncronos para chamadas de alta latência de serviço em que o pool de threads cresce para o .NET 4.5 padrão máximo de 5, threads 000 consumiria aproximadamente 5 GB mais memória do que um aplicativo capaz do a mesma das solicitações de serviço usando métodos assíncronos e somente 50 threads. Quando você estiver fazendo o trabalho assíncrono, você nem sempre está usando um thread. Por exemplo, quando você faz uma solicitação de serviço da web assíncrona, ASP.NET não usará os threads entre o **async** chamada de método e o **await**. Usar o pool de threads para atender às solicitações com alta latência pode levar a um grande volume de memória e a subutilização do hardware do servidor.

## <a name="processing-asynchronous-requests"></a>Processamento de solicitações assíncronas

Em um aplicativo web que vê um grande número de solicitações simultâneas na inicialização ou tenha uma carga de intermitente (em que o aumento da simultaneidade, de repente,), fazer chamadas de serviço web assíncrona aumenta a capacidade de resposta do aplicativo. Uma solicitação assíncrona leva a mesma quantidade de tempo de processamento como uma solicitação síncrona. Se uma solicitação faz um web chamada de serviço que requer dois segundos ser concluída, a solicitação de usa dois segundos se ela é executada de forma síncrona ou assíncrona. No entanto durante uma chamada assíncrona, um thread não é bloqueado de responder a outras solicitações enquanto espera para a primeira solicitação concluir. Portanto, as solicitações assíncronas evitar crescimento de pool de enfileiramento de mensagens e o thread de solicitação quando houver muitas solicitações simultâneas que invocar operações de longa execução.

## <a id="ChoosingSyncVasync"></a>  Escolher métodos de ação síncrono ou assíncrono

Esta seção lista as diretrizes para quando usar métodos de ação síncrono ou assíncrono. Essas são apenas diretrizes; Examine cada aplicativo individualmente para determinar se os métodos assíncronos ajudam no desempenho.

Em geral, use os métodos síncronos para as seguintes condições:

- As operações são simples ou curta execução.
- A simplicidade é mais importante do que a eficiência.
- As operações são principalmente operações de CPU em vez de operações que envolvem extensivo disco ou sobrecarga de rede. Usar métodos de ação assíncrono em operações associadas à CPU não oferece nenhum benefício e resulta em mais sobrecarga.

  Em geral, use os métodos assíncronos para as seguintes condições:

- Você está chamando serviços que podem ser consumidos por meio de métodos assíncronos, e você estiver usando o .NET 4.5 ou superior.
- As operações são vinculadas à rede ou/S vinculadas em vez de vinculada à CPU.
- Paralelismo é mais importante do que a simplicidade do código.
- Você deseja fornecer um mecanismo que permite aos usuários cancelar uma solicitação de longa execução.
- Quando o benefício da alternância de threads de supera o custo da alternância de contexto. Em geral, você deve fazer um método assíncrono se espera que o método síncrono no thread de solicitação ASP.NET ao não fazer nenhum trabalho. Fazendo a chamada assíncrona, o thread de solicitação do ASP.NET não está paralisado não fazendo nenhum trabalho enquanto ele aguarda a solicitação de serviço da web ser concluída.
- O teste mostra que o bloqueio de operações é um gargalo no desempenho do site e que o IIS pode atender mais solicitações usando métodos assíncronos para essas chamadas de bloqueio.

  O exemplo baixável mostra como usar métodos de ação assíncrono com eficiência. O exemplo fornecido foi projetado para fornecer uma demonstração simple de programação assíncrona no ASP.NET MVC 4 usando o .NET 4.5. O exemplo não pretende ser uma arquitetura de referência para programação assíncrona no ASP.NET MVC. O programa de exemplo chama [API Web ASP.NET](../../../web-api/index.md) métodos que chamam por sua vez [Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular as chamadas de serviço web de execução longa. A maioria dos aplicativos de produção não mostrará tais benefícios óbvios para usar os métodos de ação assíncrono.   
  
Alguns aplicativos exigem que todos os métodos de ação para ser assíncrona. Muitas vezes, convertendo alguns métodos de ação síncrono em métodos assíncronos fornece o melhor aumento de eficiência para a quantidade de trabalho necessário.

## <a id="SampleApp"></a>  O aplicativo de exemplo

Você pode baixar o aplicativo de exemplo do [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) sobre o [GitHub](https://github.com/) site. O repositório consiste em três projetos:

- *Mvc4Async*: projeto o ASP.NET MVC 4 que contém o código usado neste tutorial. Ele faz chamadas de API da Web para o **WebAPIpgw** service.
- *WebAPIpgw*: projeto de API da Web do ASP.NET MVC 4 que implementa o `Products, Gizmos and Widgets` controladores. Ele fornece os dados para o *WebAppAsync* project e o *Mvc4Async* projeto.
- *WebAppAsync*: projeto de Web Forms do ASP.NET a usada em outro tutorial.

## <a id="GizmosSynch"></a>  O método de ação síncrono utensílios

 O seguinte código mostra o `Gizmos` método de ação síncrono que é usado para exibir uma lista de utensílios. (Para este artigo, um gizmo é um dispositivo mecânico fictício.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

O seguinte código mostra o `GetGizmos` método do serviço gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

O `GizmoService GetGizmos` método passa um URI para um serviço ASP.NET Web API HTTP que retorna uma lista de dados de utensílios. O *WebAPIpgw* projeto contém a implementação da API Web `gizmos, widget` e `product` controladores.  
A imagem a seguir mostra a exibição de utensílios do projeto.

![Utensílios](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Criando um método de ação assíncrono utensílios

O exemplo usa o novo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavras-chave (disponível no .NET 4.5 e Visual Studio 2012) para permitir que o compilador ser responsável por manter as complicado transformações necessárias para programação assíncrona. O compilador permite que você escreva código usando que construções fluxo de controle síncrono do # e o compilador aplica automaticamente as transformações necessárias para usar retornos de chamada para evitar o bloqueio de threads.

O código a seguir mostra a `Gizmos` método síncrono e o `GizmosAsync` método assíncrono. Se o navegador dá suporte a [HTML 5 `<mark>` elemento](http://www.w3.org/wiki/HTML/Elements/mark), você verá as alterações no `GizmosAsync` no realce amarelo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 As seguintes alterações foram aplicadas para permitir que o `GizmosAsync` ser assíncrona.

- O método é marcado com o [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, que informa ao compilador para gerar retornos de chamada para partes do corpo e criar automaticamente um `Task<ActionResult>` que é retornado.
- &quot;Async&quot; foi acrescentado ao nome do método. Acrescentar "Async" não é necessário, mas é a convenção quando escrever métodos assíncronos.
- O tipo de retorno foi alterado de `ActionResult` para `Task<ActionResult>`. O tipo de retorno de `Task<ActionResult>` representa um trabalho em andamento e fornece os chamadores do método com um identificador por meio do qual aguardar a conclusão da operação assíncrona. Nesse caso, o chamador é o serviço web. `Task<ActionResult>` representa contínua funciona com o resultado `ActionResult.`
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada para a chamada de serviço web.
- A API do serviço web assíncrona foi chamada (`GetGizmosAsync`).

Dentro do `GetGizmosAsync` outro método assíncrono, de corpo de método `GetGizmosAsync` é chamado. `GetGizmosAsync` retorna imediatamente um `Task<List<Gizmo>>` que eventualmente será concluída quando os dados estão disponíveis. Porque você não quiser fazer mais nada até que você tenha os dados gizmo, o código aguarda a tarefa (usando o **await** palavra-chave). Você pode usar o **await** palavra-chave apenas em métodos anotados com o **async** palavra-chave.

O **await** palavra-chave não bloqueia o thread até que a tarefa seja concluída. Ele se inscreve o restante do método como um retorno de chamada na tarefa e retorna imediatamente. Quando a tarefa aguardada finalmente é concluído, ele invoque o retorno de chamada e, portanto, retomar a execução de à direita do método onde parou. Para obter mais informações sobre como usar o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte o [async referências](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

O código a seguir mostra a `GetGizmos` e `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 As alterações assíncronas são semelhantes às feitas para o **GizmosAsync** acima. 

- A assinatura do método foi anotada com o [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, o tipo de retorno foi alterado para `Task<List<Gizmo>>`, e *Async* foi acrescentado ao nome do método.
- Assíncrono [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe é usada em vez da [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada para o [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) métodos assíncronos.

A imagem a seguir mostra a exibição de gizmo assíncrona.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

A apresentação de navegadores dos dados utensílios é idêntica ao modo de exibição criado pela chamada síncrona. A única diferença é que a versão assíncrona pode ser mais alto de desempenho com cargas pesadas.

## <a id="Parallel"></a>  Executar várias operações em paralelo

Métodos de ação assíncrono têm uma vantagem significativa sobre métodos síncronos quando uma ação deve executar várias operações independentes. No exemplo fornecido, o método síncrono `PWG`(para produtos, Widgets e utensílios) exibe os resultados de três chamadas de serviço web para obter uma lista de produtos, widgets e utensílios. O [API Web ASP.NET](../../../web-api/index.md) de projeto que fornece esses serviços usa [Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular a latência de rede lenta a chamadas. Quando o atraso é definido como 500 milissegundos, assíncrona `PWGasync` método leva um pouco mais de 500 milissegundos seja concluída e síncronos `PWG` versão assume 1.500 milésimos de segundo. Assíncrono `PWG` método é mostrado no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Assíncrona `PWGasync` método é mostrado no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

A imagem a seguir mostra a exibição retornada do **PWGasync** método.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Usando um Token de cancelamento

Métodos de ação assíncrono retornando `Task<ActionResult>`são cancelável, é que eles terão uma [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parâmetro quando ele é fornecido com o [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) atributo. O seguinte código mostra o `GizmosCancelAsync` método com um tempo limite de 150 milissegundos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

O código a seguir mostra a sobrecarga de GetGizmosAsync, que usa um [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parâmetro.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

No aplicativo de exemplo fornecido, selecionando o *demonstração de Token de cancelamento* chamadas de vincular o `GizmosCancelAsync` método e demonstra o cancelamento da chamada assíncrona.

## <a id="ServerConfig"></a>  Configuração de servidor de alta simultaneidade/alta latência chamadas a serviços Web

Para obter os benefícios de um aplicativo da web assíncrona, você talvez precise fazer algumas alterações na configuração de servidor padrão. Mantenha o seguinte em mente ao configurar e seu aplicativo da web assíncrona de teste de carga.

- Windows 7, Windows Vista e todos os sistemas de operacionais de cliente Windows têm um máximo de 10 solicitações simultâneas. Você precisará de um sistema de operacional Windows Server para ver os benefícios de métodos assíncronos sob alta carga.
- Registre o .NET 4.5 com o IIS em um prompt de comando elevado:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
  Ver [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Talvez você precise aumentar o [HTTP. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite da fila do valor padrão de 1.000 para 5.000. Se a configuração for muito baixo, você poderá ver [HTTP. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rejeitar as solicitações com um status HTTP 503. Para alterar o limite da fila HTTP. sys:

    - Abra o Gerenciador do IIS e navegue até o painel de Pools de aplicativos.
    - Clique com o botão direito no pool de aplicativos de destino e selecione **configurações avançadas**.  
        ![advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - No **configurações avançadas** caixa de diálogo alteração *comprimento da fila* de 1.000 para 5.000.  
        ![Comprimento da fila](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Observe que nas imagens acima, o .NET framework está listado como v4.0, mesmo se o pool de aplicativos estiver usando o .NET 4.5. Para entender essa discrepância, consulte o seguinte:

    - [Controle de versão do .NET e .NET 4.5 Multi-Targeting - é uma atualização in-loco para o .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Como configurar um aplicativo do IIS ou o AppPool usar ASP.NET 3.5 em vez de 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Versões e dependências do .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Se seu aplicativo está usando serviços da web ou System.NET para se comunicar com um back-end a via HTTP, você talvez seja necessário aumentar o [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para aplicativos ASP.NET, isso é limitado pelo recurso de configuração automática de 12 vezes o número de CPUs. Isso significa que em um procedimento de quatro, você pode ter no máximo 12 \* 4 = 48 conexões simultâneas com um ponto de extremidade IP. Como isso está associado à [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), a maneira mais fácil para aumentar `maxconnection` em um ASP.NET application é definir [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) por meio de programação em a partir `Application_Start` método na *global. asax* arquivo. Veja o exemplo de download para obter um exemplo.
- No .NET 4.5, o padrão de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) deve ser o suficiente.
