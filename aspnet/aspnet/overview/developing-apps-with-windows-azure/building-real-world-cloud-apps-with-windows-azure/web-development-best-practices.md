---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: "Web práticas recomendadas de desenvolvimento (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: a40a3779ddc416e141dd27b665f43830a43590b1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Práticas recomendadas de desenvolvimento da Web (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Os três primeiros padrões foram sobre como configurar um processo de desenvolvimento ágil; o restante são sobre a arquitetura e o código. Este é um conjunto de práticas recomendadas de desenvolvimento da web:

- [Servidores web sem monitoração de estado](#stateless) atrás de um balanceador de carga inteligente.
- [Evite o estado de sessão](#sessionstate) (ou se você não pode evitar isso, use o cache distribuído, em vez de um banco de dados).
- [Usar um CDN](#cdn) a ativos de arquivo estático do cache de borda (imagens, scripts).
- [Use o suporte assíncrono do .NET 4.5](#async) para evitar o bloqueio de chamadas.

Essas práticas são válidas para todo o desenvolvimento da web, não apenas para aplicativos em nuvem, mas são especialmente importantes para aplicativos de nuvem. Eles funcionam juntos para ajudá-lo a fazer o melhor uso do dimensionamento altamente flexível oferecidos pelo ambiente de nuvem. Se você não seguir essas práticas, você terá limitações quando você tentar expandir seu aplicativo.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Camada da web sem monitoração de estado por trás de um balanceador de carga inteligente

*Camada da web sem monitoração de estado* significa que você não armazene os dados de aplicativo no sistema do servidor web memória ou o arquivo. Manter a camada da web sem monitoração de estado permite que você fornecer uma melhor experiência de cliente e economizar dinheiro:

- Se a camada da web é sem monitoração de estado e está atrás de um balanceador de carga, você pode responder rapidamente a alterações no tráfego do aplicativo adicionando ou removendo servidores dinamicamente. No ambiente de nuvem em que você paga apenas pelos recursos de servidor para desde que você realmente usa essa capacidade para responder a mudanças na demanda pode se transformar em grande economia.
- Uma camada da web sem monitoração de estado é arquitetonicamente muito mais simples expandir o aplicativo. Que muito permite que você responder ao dimensionamento necessidades mais rápido e menos gastar em desenvolvimento e teste do processo.
- Servidores de nuvem, como servidores de locais, precisam ser corrigidos e reinicializado ocasionalmente; e se a camada da web sem monitoração de estado, redirecionar tráfego quando um servidor ficar temporariamente não causará erros ou um comportamento inesperado.

A maioria dos aplicativos do mundo real necessário armazenar o estado de uma sessão da web; o ponto principal aqui é não armazená-lo no servidor web. Você pode armazenar o estado de outras maneiras, como no cliente em cookies ou fora do servidor de processo no estado da sessão ASP.NET usando um provedor de cache. Você pode armazenar arquivos em [armazenamento de Blob do Windows Azure](unstructured-blob-storage.md) em vez do sistema de arquivos local.

Como um exemplo de como é fácil dimensionar um aplicativo no Windows Azure Web Sites, se a camada da web é sem monitoração de estado, consulte o **escala** guia para um Site Windows Azure Web no portal de gerenciamento:

![Guia de escala](web-development-best-practices/_static/image1.png)

Se você quiser adicionar servidores web, você apenas pode arrastar o controle deslizante contagem de instância para a direita. Defina-a como 5 e clique em **salvar**, e dentro de segundos que 5 servidores de web no Windows Azure tratamento do tráfego do site.

![Cinco instâncias](web-development-best-practices/_static/image2.png)

Facilmente, você pode definir a contagem de instâncias até 3 ou volta até 1. Quando você escala back, iniciar economia imediatamente porque o Windows Azure cobra por minuto, não por hora.

Você também pode instruir o Windows Azure para aumentar ou diminuir o número de servidores web com base no uso da CPU de automaticamente. No exemplo a seguir, quando o uso da CPU for inferior a 60%, o número de servidores web será reduzido ao mínimo de 2 e, se o uso da CPU ficar acima de 80%, o número de servidores web será aumentado até um máximo de 4.

![Dimensionar pelo uso da CPU](web-development-best-practices/_static/image3.png)

Ou, se você souber que seu site só será ocupado durante o horário comercial? Você pode informar o Windows Azure para executar vários servidores durante o dia e diminuir a um único servidor evenings, nights e fins de semana. A série de capturas de tela a seguir mostra como configurar o site da web para executar um servidor fora do horário comercial e 4 servidores durante o horário de trabalho de 8: 00 a 17: 00.

![Escala por agenda](web-development-best-practices/_static/image4.png)

![Definir tempos de agenda](web-development-best-practices/_static/image5.png)

![Agendamento de dia](web-development-best-practices/_static/image6.png)

![Agendamento de weeknight](web-development-best-practices/_static/image7.png)

![Agendamento de fim de semana](web-development-best-practices/_static/image8.png)

E claro tudo isso pode ser feito em scripts, bem como o portal.

A capacidade de seu aplicativo de expansão é quase ilimitada no Windows Azure, desde que você evite impedimentos dinamicamente adicionando ou removendo VMs de servidor, mantendo a camada da web sem monitoração de estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evite o estado da sessão

Geralmente não é prático em um aplicativo de nuvem do mundo real para evitar o armazenamento de alguma forma de estado para uma sessão de usuário, mas algumas abordagens afetam o desempenho e escalabilidade mais do que outras pessoas. Se você precisar armazenar o estado, a melhor solução é manter a quantidade de estado menor e armazená-lo em cookies. Se isso não for viável, a próxima melhor solução é usar o estado da sessão ASP.NET com um provedor de [cache distribuído na memória](distributed-caching.md#sessionstate). A pior solução de um ponto de vista de escalabilidade e o desempenho é usar um banco de dados feito o provedor de estado de sessão.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar um CDN para armazenar em cache os ativos de arquivo estático

CDN é um acrônimo para rede de fornecimento de conteúdo. Fornecer recursos de arquivo estático, como imagens e arquivos de script para um provedor CDN, e o provedor armazena esses arquivos em data centers em todo o mundo para que sempre que as pessoas acessarem seu aplicativo, eles obtém resposta relativamente rápida e baixa latência para o cache ativos. Isso acelera o tempo de carregamento geral do site e reduz a carga nos servidores web. As CDNs são especialmente importantes se você está acessando um público que é amplamente distribuído geograficamente.

Windows Azure tem um CDN, e você pode usar outras CDNs em um aplicativo que é executado no Windows Azure ou em qualquer ambiente de hospedagem de web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Use o suporte assíncrono do .NET 4.5 para evitar o bloqueio de chamadas

.NET 4.5 aprimorado linguagens de programação c# e VB para torná-lo muito mais simples de lidar com tarefas de forma assíncrona. O benefício de programação assíncrona não é apenas para situações de processamento paralelo, como quando você deseja iniciar simultaneamente várias chamadas de serviço web. Ele também permite que o servidor web para executar de forma mais eficiente e confiável em condições de carga alta. Um servidor web tem apenas um número limitado de threads disponíveis, e em condições de carga alta quando todos os threads estão em uso, solicitações de entrada precisam aguardar até que os threads são liberados. Se o código do aplicativo não lidar com tarefas como banco de dados consultas e chamadas de serviço web assincronamente, muitos threads estão vinculados desnecessariamente enquanto o servidor está aguardando uma resposta de e/s. Isso limita a quantidade de tráfego que o servidor pode manipular em condições de carga alta. Com a programação assíncrona, threads que estão aguardando um serviço web ou um banco de dados para retornar os dados são liberados para atender novas solicitações até que os dados a é recebida. Em um servidor web ocupado, centenas ou milhares de solicitações podem ser processadas imediatamente que outra forma seria esperando por threads para ser liberado.

Como você viu anteriormente, é mais fácil de reduzir o número de servidores de web tratamento seu site da web, como ele é aumentá-las. Então se um servidor pode alcançar maior taxa de transferência, você não precisa como muitos deles, e você podem reduzir os custos porque você precisa de servidores para um volume de tráfego de determinada menos do que outra forma.

Suporte para o modelo de programação assíncrono do .NET 4.5 está incluído no ASP.NET 4.5 para Web Forms, MVC e API da Web; no Entity Framework 6 e no [API de armazenamento do Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Suporte assíncrono no ASP.NET 4.5

No ASP.NET 4.5, o suporte para programação assíncrona tenha sido adicionada, não apenas para o idioma, mas também para as estruturas de API da Web, Web Forms e MVC. Por exemplo, um método de ação do controlador MVC do ASP.NET recebe dados de uma solicitação da web e transfere os dados para uma exibição que cria o HTML a ser enviado para o navegador. Com frequência, o método de ação precisa obter dados de um banco de dados ou serviço web para exibi-lo em uma página da web ou salvar os dados inseridos em uma página da web. Esses cenários é fácil de fazer o método de ação assíncrono: em vez de retornar um *ActionResult* do objeto, você retornar *tarefa&lt;ActionResult&gt;*  e marcar o método com o *async* palavra-chave. Dentro do método, quando uma linha de código inicia uma operação que envolve o tempo de espera, você marcá-la com a palavra-chave await.

Este é um método de ação simples que chama um método de repositório para uma consulta de banco de dados:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

E aqui está o mesmo método que trata a chamada de banco de dados assíncrona:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Nos bastidores, o compilador gera o código apropriado seja assíncrono. Quando o aplicativo faz a chamada para `FindTaskByIdAsync`, o ASP.NET torna o `FindTask` solicitar esvazia o thread de trabalho e o torna disponível para processar outra solicitação. Quando o `FindTask` solicitação é feita, um thread é reiniciado para continuar a processar o código que segue a chamada. Durante a verificação provisória entre quando o `FindTask` solicitação é iniciada e quando os dados são retornados, você tem um thread disponível para fazer o trabalho útil que, normalmente, poderia ser parado aguardando a resposta.

Há alguma sobrecarga para código assíncrono, mas em condições de baixa carga, essa sobrecarga é insignificante, ao mesmo tempo sob condições de alta carga que você é capaz de processar solicitações caso contrário, seriam mantidas aguardando threads disponíveis.

Foi possível fazer esse tipo de programação assíncrona desde o ASP.NET 1.1, mas era difícil de escrever, propensas a erros e difíceis de depurar. Agora que Simplificamos a codificação para ele no ASP.NET 4.5, não há nenhuma razão para não fazê-lo mais.

### <a name="async-support-in-entity-framework-6"></a>Suporte assíncrono no Entity Framework 6

Como parte do suporte assíncrono 4.5 enviamos o suporte assíncrono para chamadas de serviço web, soquetes e sistema de arquivos e/s, mas é o padrão mais comum para aplicativos da web atingir um banco de dados e bibliotecas nossos dados não oferece suporte à assíncrono. Agora o Entity Framework 6 adiciona o suporte assíncrono para acesso ao banco de dados.

No Entity Framework 6 todos os métodos que causam uma consulta ou um comando a ser enviado para o banco de dados tem versões assíncronas. Este exemplo mostra a versão assíncrona do *localizar* método.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

E esse suporte assíncrono funciona não apenas para inserções, exclusões, atualizações e localiza simple, também funciona com consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Há um `Async` versão do `ToList` método porque esse código é o método que faz com que uma consulta a ser enviado para o banco de dados. O `Where` e `OrderByDescending` métodos apenas configuram a consulta, enquanto o `ToListAsync` método executa a consulta e armazena a resposta no `result` variável.

## <a name="summary"></a>Resumo

Você pode implementar as práticas recomendadas de desenvolvimento web descritas aqui em qualquer estrutura e um ambiente de nuvem de programação da web, mas temos ferramentas do ASP.NET e do Windows Azure para tornar mais fácil. Se você seguir esses padrões, você pode facilmente dimensionar a camada da web, e você vai minimizar as despesas porque cada servidor será capaz de lidar com mais tráfego.

O [próximo capítulo](single-sign-on.md) examina como nuvem permite cenários de logon único.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Servidores web sem monitoração de estado:

- [Microsoft Patterns e práticas – diretrizes de dimensionamento automático](https://msdn.microsoft.com/en-us/library/dn589774.aspx).
- [Afinidade no Windows Azure Web Sites ARR desabilitação da instância](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Postagem no blog por Erez Benari, explica a afinidade de sessão no Windows Azure Web Sites.

CDN:

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Consulte a discussão CDN episódio 3 começando em 1:34:00.
- [Microsoft Patterns e práticas recomendadas estático hospedagem de conteúdo padrão](https://msdn.microsoft.com/en-us/library/dn589776.aspx)
- [Revisões CDN](http://www.cdnreviews.com/). Visão geral dos muitos CDNs.

Programação assíncrona:

- [Usando métodos assíncronos no ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programação assíncrona com Async e Await (c# e Visual Basic)](https://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx). MSDN white paper que explica a lógica de programação assíncrona, como ele funciona no ASP.NET 4.5 e como escrever código para implementá-la.
- [Consulta do Entity Framework assíncrona e salvar](https://msdn.microsoft.com/en-us/data/jj819165)
- [Como criar aplicativos da Web do ASP.NET usando o Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Apresentação em vídeo por Rowan Miller. Inclui uma demonstração de gráfico de como a programação assíncrona pode facilitar a enorme aumento na taxa de transferência do servidor web sob condições de alta carga.
- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Para discussões sobre o impacto de programação assíncrona em escalabilidade, consulte episódio 4 e episódio 8.
- [A mágica do uso de métodos assíncronos no ASP.NET 4.5 mais um problema importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Postagem no blog por Scott Hanselman, principalmente sobre usando async em aplicativos Web Forms do ASP.NET.

Para práticas recomendadas de desenvolvimento de web adicionais, consulte os seguintes recursos:

- [A correção-exemplo de aplicativo - práticas recomendadas](the-fix-it-sample-application.md#bestpractices). Apêndice para este livro eletrônico lista um conjunto de práticas recomendadas que foram implementadas no aplicativo corrigi-lo.
- [Lista de verificação de desenvolvedor da Web](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[Anterior](continuous-integration-and-continuous-delivery.md)
[Próximo](single-sign-on.md)
