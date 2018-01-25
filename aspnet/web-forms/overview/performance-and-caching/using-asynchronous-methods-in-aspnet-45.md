---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "Usando métodos assíncronos no ASP.NET 4.5 | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo ASP.NET Web Forms assíncrono usando o Visual Studio Express 2012 para Web, que é um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 73e46134cfafb9edc4c1888211eab44b8f2bf828
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Usando métodos assíncronos no ASP.NET 4.5
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo as Noções básicas de criação de um aplicativo ASP.NET Web Forms assíncrona usando [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11), que é uma versão gratuita do Microsoft Visual Studio. Você também pode usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). As seções a seguir estão incluídas neste tutorial.
> 
> - [Como as solicitações são processadas pelo Pool de Thread](#HowRequestsProcessedByTP)
> - [Escolha os métodos síncronos ou assíncronos](#ChoosingSyncVasync)
> - [O aplicativo de exemplo](#SampleApp)
> - [A página síncrona largue as](#GizmosSynch)
> - [Criando uma página largue as assíncrona](#CreatingAsynchGizmos)
> - [Executar várias operações em paralelo](#Parallel)
> - [Usando um Token de cancelamento](#CancelToken)
> - [Configuração do servidor para o serviço Web de alta simultaneidade/alta latência chamadas](#ServerConfig)
> 
> Um exemplo é fornecido para este tutorial em  
>  [https://GitHub.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) no [GitHub](https://github.com/) site.


Páginas da Web ASP.NET 4.5 em combinação [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permite que você registre os métodos assíncronos que retornam um objeto do tipo [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). O .NET Framework 4 introduziu um conceito de programação assíncrono conhecido como um [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) e dá suporte ao ASP.NET 4.5 [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Tarefas são representadas pelo **tarefa** tipo e tipos relacionados no [Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) namespace. O .NET Framework 4.5 amplia esse suporte assíncrono com o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave que tornam o trabalho com [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objetos muito menos complexos do que o anterior métodos assíncronos. O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave é a abreviação sintática para indicar que um trecho de código deve aguardar assincronamente em alguma outra parte do código. O [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave representa uma dica que você pode usar para marcar métodos como métodos assíncronos baseado em tarefa. A combinação de **await**, **async**e o **tarefa** objeto torna muito mais fácil para escrever código assíncrono no .NET 4.5. O novo modelo para métodos assíncronos é chamado de *padrão assíncrono baseado em tarefa* (**toque**). Este tutorial presume que você tenha alguma familiaridade com programação assíncrona usando [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace.

Para obter mais informações sobre o uso de [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte as seguintes referências.

- [White paper: Assincronia no .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Perguntas Frequentes de Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programação assíncrona do Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Como as solicitações são processadas pelo Pool de Thread

No servidor web, o .NET Framework mantém um pool de threads que são usados para atender a solicitações ASP.NET. Quando uma solicitação chega, um thread do pool é enviado para processar essa solicitação. Se a solicitação é processada de forma síncrona, o thread que processa a solicitação está ocupado durante a solicitação está sendo processado e que o thread não pode atender outra solicitação.   
  
Isso não pode ser um problema, porque o pool de threads pode se tornar grande o suficiente para acomodar o número de threads ocupado. No entanto, o número de threads no pool de threads é limitado (o padrão máximo para o .NET 4.5 é 5.000). Em aplicativos grandes com simultaneidade alta de solicitações de execução longa, todos os threads disponíveis podem estar ocupados. Essa condição é conhecida como escassez de threads. Quando essa condição for atingida, o servidor web enfileira as solicitações. Se a fila de solicitações ficar cheio, o servidor web rejeita solicitações com um status de HTTP 503 (servidor muito ocupado). O pool de threads do CLR tem limitações na nova injeções de thread. Se a simultaneidade é intermitente (isto é, seu site da web pode repentinamente obter um grande número de solicitações) e todos os threads de solicitação disponíveis estão ocupados devido a chamadas de back-end com alta latência, a taxa de injeção de thread limitado pode tornar seu aplicativo mal responder. Além disso, cada novo thread adicionado ao pool de threads tem sobrecarga (por exemplo, 1 MB de memória de pilha). Um aplicativo web usando métodos síncronos a chamadas de alta latência de serviço em que o pool de threads aumenta para o .NET 4.5 padrão máximo de 5, 000 threads consumiria aproximadamente 5 GB mais memória do que um aplicativo capaz de iguais as solicitações de serviço usando métodos assíncronos e apenas 50 threads. Quando você estiver fazendo o trabalho assíncrona, você nem sempre está usando um thread. Por exemplo, quando você faz uma solicitação de serviço da web assíncrono, ASP.NET não usará a threads entre o **async** chamada de método e o **await**. Usar o pool de threads para solicitações de serviço com alta latência, isso pode levar a um espaço de memória grandes e a baixa utilização do hardware do servidor.

## <a name="processing-asynchronous-requests"></a>Processamento de solicitações assíncronas

Em aplicativos web que vê um grande número de solicitações simultâneas na inicialização ou tem uma carga intermitente (onde simultaneidade aumenta, de repente,), fazer chamadas de serviço web assíncrona aumentará a capacidade de resposta do seu aplicativo. Uma solicitação assíncrona leva a mesma quantidade de tempo para processar uma solicitação síncrona. Por exemplo, se uma solicitação de um serviço web chamar que requer dois segundos ser concluída, a solicitação usa dois segundos se ela é realizada de forma síncrona ou assíncrona. No entanto, durante uma chamada assíncrona, um thread não é bloqueado de responder às outras solicitações enquanto aguarda a primeira solicitação ser concluída. Portanto, solicitações assíncronas evitar o crescimento de pool de enfileiramento de mensagens e thread de solicitação quando há muitas solicitações simultâneas que invocar operações de execução longa.

## <a id="ChoosingSyncVasync"></a>Escolha os métodos síncronos ou assíncronos

Esta seção lista as diretrizes para quando usar métodos síncronos ou assíncronos. Essas são apenas diretrizes; Examine cada aplicativo individualmente para determinar se os métodos assíncronos ajudam com o desempenho.

Em geral, use os métodos síncronos para as seguintes condições:

- As operações são simples ou curta execução.
- Simplicidade é mais importante do que a eficiência.
- As operações são principalmente as operações da CPU, em vez de operações que envolvem a sobrecarga de rede ou de disco abrangentes. Usando métodos assíncronos em operações associadas à CPU não fornece nenhuma benefícios e resulta em mais sobrecarga.

 Em geral, use os métodos assíncronos para as seguintes condições:

- Chamada de serviços que podem ser consumidos por meio de métodos assíncronos, e você estiver usando o .NET 4.5 ou posterior.
- As operações são vinculadas à rede ou I/O vinculados em vez de limite de CPU.
- Paralelismo é mais importante do que a simplicidade do código.
- Você deseja fornecer um mecanismo que permite aos usuários cancelar uma solicitação de longa execução.
- Quando o benefício da comutação threads out pondera o custo da alternância de contexto. Em geral, você deve fazer um método assíncrono se o método síncrono bloqueia o thread de solicitação do ASP.NET ao não fazer nenhum trabalho. Fazendo a chamada assíncrona, o thread de solicitação do ASP.NET não está bloqueado não executando nenhum trabalho enquanto aguarda a solicitação de serviço da web concluir.
- O teste mostra que o bloqueio de operações é um afunilamento no desempenho do site e que o IIS pode atender mais solicitações usando métodos assíncronos para essas chamadas de bloqueio.

 O exemplo disponível para download mostra como usar métodos assíncronos com eficiência. O exemplo fornecido foi projetado para fornecer uma demonstração simple de programação assíncrona no ASP.NET 4.5. O exemplo não pretende ser uma arquitetura de referência para programação assíncrona no ASP.NET. O programa de exemplo chama [ASP.NET Web API](../../../web-api/index.md) métodos que por sua vez chamam [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular chamadas de serviço web de longa execução. A maioria dos aplicativos de produção não mostrará esses benefícios óbvios ao uso de métodos assíncronos.   
  
Alguns aplicativos exigem que todos os métodos para ser assíncrona. Geralmente, a conversão de alguns métodos síncronos para métodos assíncronos fornece o aumento de eficiência melhor para a quantidade de trabalho necessário.

## <a id="SampleApp"></a>O aplicativo de exemplo

Você pode baixar o aplicativo de exemplo do [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) no [GitHub](https://github.com/) site. O repositório consiste em três projetos:

- *WebAppAsync*: projeto o ASP.NET Web Forms que consome a API da Web **WebAPIpwg** service. A maior parte do código para este tutorial é neste projeto.
- *WebAPIpgw*: projeto de API da Web do ASP.NET MVC 4 que implementa o `Products, Gizmos and Widgets` controladores. Ele fornece os dados para o *WebAppAsync* projeto e o *Mvc4Async* projeto.
- *Mvc4Async*: projeto o ASP.NET MVC 4 que contém o código usado em outro tutorial. Ele faz chamadas de API da Web para o **WebAPIpwg** service.

## <a id="GizmosSynch"></a>A página síncrona largue as

 O código a seguir mostra o `Page_Load` método síncrono é usado para exibir uma lista de largue as. (Neste artigo, um gizmo é um dispositivo mecânico fictício.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

O código a seguir mostra o `GetGizmos` método do serviço gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

O `GizmoService GetGizmos` método passa um URI para um serviço ASP.NET Web API HTTP que retorna uma lista de dados largue as. O *WebAPIpgw* projeto contém a implementação da API da Web `gizmos, widget` e `product` controladores.  
A imagem a seguir mostra a página largue as o projeto de exemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Criando uma página largue as assíncrona

O exemplo usa o novo [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) e [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavras-chave (disponível no .NET 4.5 e o Visual Studio 2012) para permitir que o compilador serão responsáveis por manter as transformações complicadas necessárias para programação assíncrona. O compilador permite que você escreva o código usando que fluxo de controle síncrono do # constrói e o compilador aplica automaticamente as transformações necessárias para usar retornos de chamada para evitar o bloqueio de threads.

As páginas ASP.NET assíncronas devem incluir o [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) diretiva com o `Async` atributo definido como "true". O código a seguir mostra o [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) diretiva com o `Async` atributo definido como "true" para o *GizmosAsync.aspx* página.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

O código a seguir mostra o `Gizmos` síncrona `Page_Load` método e o `GizmosAsync` página assíncrona. Se o seu navegador suporta o [HTML 5 &lt;marcar&gt; elemento](http://www.w3.org/wiki/HTML/Elements/mark), você verá as alterações no `GizmosAsync` no realce amarela.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

A versão assíncrona:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 As seguintes alterações foram aplicadas para permitir que o `GizmosAsync` página ser assíncrona.

- O [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) diretiva deve ter o `Async` atributo definido como "true".
- O `RegisterAsyncTask` método é usado para registrar uma tarefa assíncrona que contém o código que é executado de forma assíncrona.
- O novo `GetGizmosSvcAsync` método está marcado com o [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, que informa ao compilador para gerar retornos de chamada para partes do corpo e criar automaticamente um `Task` que é retornado.
- &quot;Assíncrono&quot; foi acrescentado ao nome do método assíncrono. Anexar "Async" não é necessário, mas é a convenção ao escrever métodos assíncronos.
- O tipo de retorno do novo novo `GetGizmosSvcAsync` método é `Task`. O tipo de retorno `Task` representa um trabalho contínuo e fornece chamadores do método com um identificador por meio do qual a aguardar pela conclusão da operação assíncrona.
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada a chamada de serviço web.
- A API do serviço da web assíncrono foi chamada (`GetGizmosAsync`).

Dentro do `GetGizmosSvcAsync` outro método assíncrono, o corpo do método `GetGizmosAsync` é chamado. `GetGizmosAsync`retorna imediatamente um `Task<List<Gizmo>>` que eventualmente será concluída quando os dados estão disponíveis. Porque você não quiser fazer mais nada até que você tenha os dados de gizmo, o código aguarda a tarefa (usando o **await** palavra-chave). Você pode usar o **await** palavra-chave somente em métodos anotados com o **async** palavra-chave.

O **await** palavra-chave não bloqueia o thread até que a tarefa seja concluída. Ele se inscreve o restante do método como um retorno de chamada da tarefa e retorna imediatamente. Quando a tarefa esperada eventualmente for concluído, ele chamar o retorno de chamada e, assim, retomar a execução do direito de método em que parou. Para obter mais informações sobre como usar o [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) e [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavras-chave e o [tarefa](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) namespace, consulte o [async referências](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

O código a seguir mostra o `GetGizmos` e `GetGizmosAsync` métodos.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 As alterações assíncronas são semelhantes às feitas a **GizmosAsync** acima. 

- A assinatura do método foi anotada com a [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) palavra-chave, o tipo de retorno foi alterado para `Task<List<Gizmo>>`, e *Async* foi acrescentado ao nome do método.
- O assíncrona [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe é usada em vez de síncrona [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- O [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) palavra-chave foi aplicada para o [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) método assíncrono.

A imagem a seguir mostra a exibição de gizmo assíncrona.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

A apresentação de navegadores dos dados largue as é idêntica ao modo de exibição criado pela chamada síncrona. A única diferença é que a versão assíncrona pode ser um desempenho melhor com cargas pesadas.

## <a name="registerasynctask-notes"></a>Notas de RegisterAsyncTask

Métodos vinculada com `RegisterAsyncTask` será executado imediatamente após [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Você também pode usar eventos de página void assíncronos diretamente, conforme mostrado no código a seguir:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

A desvantagem eventos void assíncronos é que os desenvolvedores não tem controle total sobre quando os eventos são executados. Por exemplo, se um. aspx e um. Definir mestre `Page_Load` eventos e um ou ambos os parâmetros são assíncronos, a ordem de execução não pode ser garantida. A mesma ordem de indeterminiate para manipuladores de eventos não (como `async void Button_Click` ) se aplica. Para a maioria dos desenvolvedores, isso deve ser aceitável, mas aqueles que exigem o controle total sobre a ordem de execução só devem usar APIs como `RegisterAsyncTask` que consumir os métodos que retornam um objeto de tarefa.

## <a id="Parallel"></a>Executar várias operações em paralelo

Métodos assíncronos tem uma vantagem significativa sobre métodos síncronos quando uma ação deve executar várias operações independentes. No exemplo fornecido, a página síncrona *PWG.aspx*(para produtos, Widgets e largue as) exibe os resultados de três chamadas de serviço da web para obter uma lista de produtos, widgets e largue as. O [ASP.NET Web API](../../../web-api/index.md) projeto que fornece esses serviços usa [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular a latência de rede lenta a chamadas. Quando o atraso é definido como 500 milissegundos, assíncronos *PWGasync.aspx* página leva um pouco mais de 500 milissegundos para concluir durante síncronos `PWG` versão assume 1.500 milésimos de segundo. Síncronos *PWG.aspx* página é mostrada no código a seguir.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

O assíncrona `PWGasync` code-behind é mostrado abaixo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

A imagem a seguir mostra a exibição retornada de assíncronos *PWGasync.aspx* página.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Usando um Token de cancelamento

Métodos assíncronos retornando `Task`são anulável, que é que eles terão uma [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parâmetro quando é fornecida com o `AsyncTimeout` atributo do [página](https://msdn.microsoft.com/library/ydy4x04a.aspx) diretiva. O código a seguir mostra o *GizmosCancelAsync.aspx* página com um tempo limite de segundo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

O código a seguir mostra o *GizmosCancelAsync.aspx.cs* arquivo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

O aplicativo de exemplo fornecido, selecionando o *GizmosCancelAsync* link chamadas a *GizmosCancelAsync.aspx* página e demonstra o cancelamento (por tempo limite) da chamada assíncrona. Como o tempo de espera está em um intervalo aleatório, talvez seja necessário atualizar a página duas vezes para obter a mensagem de erro de tempo limite.

## <a id="ServerConfig"></a>Configuração do servidor para o serviço Web de alta simultaneidade/alta latência chamadas

Para obter os benefícios de um aplicativo web assíncrona, você precisará fazer algumas alterações na configuração de servidor padrão. Tenha em mente ao configurar e testar seu aplicativo web assíncrona de estresse.

- Windows 7, Windows Vista, Windows 8 e todos os sistemas de operacionais de cliente Windows ter um máximo de 10 solicitações simultâneas. Você precisará de um sistema operacional de servidor do Windows para ver os benefícios dos métodos assíncronos sob alta carga.
- Registre o .NET 4.5 com o IIS em um prompt de comando elevado usando o seguinte comando:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 Consulte [ferramenta de registro ASP.NET IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Talvez seja necessário aumentar o [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite da fila do valor padrão de 1.000 a 5.000. Se a configuração é muito baixa, você poderá ver [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rejeitar solicitações com um status de HTTP 503. Para alterar o limite de fila de HTTP. sys:

    - Abra o Gerenciador do IIS e navegue até o painel de Pools de aplicativos.
    - Clique com o botão direito no pool de aplicativos de destino e selecione **configurações avançadas**.  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - No **configurações avançadas** caixa de diálogo Alterar *comprimento da fila de* mais de 1.000 a 5.000.  
        ![Comprimento da fila](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Observe que nas imagens acima, o .NET framework está listado como v 4.0, mesmo que o pool de aplicativos usando o .NET 4.5. Para entender essa discrepância, consulte o seguinte:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Se seu aplicativo estiver usando serviços web ou System.NET para se comunicar com um back-end a via HTTP, você pode precisar aumentar a [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elemento. Para aplicativos ASP.NET, isso é limitado pelo recurso de configuração automática para 12 vezes o número de CPUs. Isso significa que em um processo quad, você pode ter no máximo 12 \* 4 = 48 conexões simultâneas com um ponto de extremidade do IP. Porque isso está vinculado ao [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), a maneira mais fácil para aumentar `maxconnection` em um ASP.NET aplicativo é definir [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programaticamente no o de `Application_Start` método o *global. asax* arquivo. Consulte o exemplo de download para obter um exemplo.
- No .NET 4.5, o padrão de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) deve ser suficiente.

## <a name="contributors"></a>Colaboradores

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
