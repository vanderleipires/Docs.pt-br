---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web práticas recomendadas de desenvolvimento (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: be014eabcc7f88f229722b2d565262c9025c5ad3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825594"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Práticas recomendadas de desenvolvimento da Web (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Os três primeiros padrões foram sobre como configurar um processo de desenvolvimento ágil; o restante é sobre a arquitetura e o código. Esta é uma coleção de melhores práticas de desenvolvimento na web:

- [Servidores web sem monitoração de estado](#stateless) atrás de um balanceador de carga inteligente.
- [Evite o estado de sessão](#sessionstate) (ou se você não é possível evitá-lo, use o cache distribuído em vez de um banco de dados).
- [Usar uma CDN](#cdn) para ativos de arquivos estáticos de cache de borda (imagens, scripts).
- [Usar o suporte a async do .NET 4.5](#async) para evitar o bloqueio de chamadas.

Essas práticas são válidas para todo o desenvolvimento da web, não apenas para aplicativos de nuvem, mas elas são especialmente importantes para aplicativos de nuvem. Eles funcionam juntos para ajudá-lo a fazer o melhor uso do dimensionamento altamente flexível oferecidos pelo ambiente de nuvem. Se você não seguir essas práticas, você executará em limitações quando você tentar dimensionar seu aplicativo.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Camada da web sem monitoração de estado por trás de um balanceador de carga inteligente

*Camada da web sem monitoração de estado* significa que você não armazenar nenhum dado de aplicativo no sistema do servidor web memória ou o arquivo. Manter sua camada da web sem monitoração de estado permite que você fornecer uma melhor experiência de cliente e economizar dinheiro:

- Se a camada da web é sem monitoração de estado e ele estiver atrás de um balanceador de carga, você pode responder rapidamente às alterações no tráfego do aplicativo adicionando ou removendo servidores dinamicamente. No ambiente de nuvem em que você paga apenas pelos recursos de servidor para desde que você realmente usar essa capacidade de responder a alterações na demanda pode ser traduzidos em grande economia.
- Uma camada da web sem monitoração de estado é em termos de arquitetura muito mais simples de escalar horizontalmente o aplicativo. Que também permite que você responder às necessidades de dimensionamento mais rapidamente e gastar menos dinheiro em desenvolvimento e teste no processo.
- Servidores de nuvem, como servidores locais, precisam ser corrigidos e reinicializadas ocasionalmente; e se a camada da web sem monitoração de estado, reencaminhamento tráfego quando um servidor fica inativo temporariamente não causará erros ou comportamento inesperado.

A maioria dos aplicativos do mundo real é necessário armazenar o estado de uma sessão da web; o ponto principal aqui é não armazená-lo no servidor web. Você pode armazenar o estado de outras maneiras, como no cliente em cookies ou fora do servidor de processo no estado de sessão ASP.NET usando um provedor de cache. Você pode armazenar arquivos no [armazenamento de Blob do Windows Azure](unstructured-blob-storage.md) em vez do sistema de arquivos local.

Como um exemplo de como é fácil dimensionar um aplicativo no Windows Azure Web Sites se sua camada da web é sem monitoração de estado, consulte a **escala** guia para um Site Windows Azure Web no portal de gerenciamento:

![Guia escala](web-development-best-practices/_static/image1.png)

Se você quiser adicionar servidores web, você pode simplesmente arrastar o controle deslizante contagem de instância à direita. Defina-o como 5 e clique em **salvar**, e dentro de segundos, você tem 5 servidores web no tratamento do tráfego do site do Windows Azure.

![Cinco instâncias](web-development-best-practices/_static/image2.png)

Facilmente, você pode definir a contagem de instâncias para baixo até 3 ou volta para 1. Quando você dimensiona back, você começa a poupar imediatamente porque o Windows Azure cobra por minuto, não por hora.

Você também pode informar ao Windows Azure para aumentar ou diminuir o número de servidores web com base no uso da CPU automaticamente. No exemplo a seguir, quando uso da CPU fica abaixo de 60%, o número de servidores web diminuirá em um mínimo de 2 e, se o uso da CPU ficar acima de 80%, o número de servidores web será aumentado até um máximo de 4.

![Dimensionar por uso de CPU](web-development-best-practices/_static/image3.png)

Ou, e se você souber que seu site só poderá ser ocupado durante o horário comercial? Você pode informar o Windows Azure para executar vários servidores durante o dia e diminuir para um único servidor evenings, nights e finais de semana. A série de capturas de tela a seguir mostra como configurar o site da web para executar um servidor em horários alternativos e servidores de 4 durante as horas de trabalho de AM 8 para às 17H.

![Escala por agenda](web-development-best-practices/_static/image4.png)

![Definir tempos de agendamento](web-development-best-practices/_static/image5.png)

![Agenda comercial](web-development-best-practices/_static/image6.png)

![Agendamento de weeknight](web-development-best-practices/_static/image7.png)

![Agendamento de fim de semana](web-development-best-practices/_static/image8.png)

E claro tudo isso pode ser feito em scripts, bem como no portal.

A capacidade do seu aplicativo para escalar horizontalmente é quase ilimitada no Windows Azure, desde que você evite impedimentos dinamicamente adicionando ou removendo VMs de servidor, mantendo a camada da web sem monitoração de estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evite o estado de sessão

Muitas vezes não é prático em um aplicativo de nuvem do mundo real para evitar o armazenamento de alguma forma de estado para uma sessão de usuário, mas algumas abordagens afetam o desempenho e escalabilidade mais do que outros. Se você tiver que armazenar o estado, a melhor solução é manter a quantidade de estado de pequeno e armazená-lo em cookies. Se isso não for viável, a próxima solução melhor é usar o estado de sessão ASP.NET com um provedor para [cache distribuído na memória](distributed-caching.md#sessionstate). A pior solução de um ponto de vista de escalabilidade e desempenho é usar um banco de dados feito o provedor de estado de sessão.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar uma CDN para armazenar em cache os ativos de arquivos estáticos

A CDN é um acrônimo para rede de distribuição de conteúdo. Você fornece ativos de arquivos estáticos, como imagens e arquivos de script para um provedor de CDN e o provedor armazena esses arquivos em data centers em todo o mundo para que sempre que as pessoas acessarem seu aplicativo, eles obtém resposta relativamente rápida e baixa latência para o armazenados em cache ativos. Isso acelera o tempo geral de carregamento do site e reduz a carga em seus servidores web. As CDNs são especialmente importantes se você alcançará um público que é amplamente distribuído geograficamente.

Windows Azure tem uma CDN, e você pode usar outras CDNs em um aplicativo que é executado no Windows Azure ou em qualquer ambiente de hospedagem de web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usar o suporte a async do .NET 4.5 para evitar o bloqueio de chamadas

.NET 4.5 aprimorada as linguagens de programação em C# e VB para torná-lo muito mais simples lidar com tarefas de forma assíncrona. O benefício da programação assíncrona não serve apenas para situações de processamento paralelo, como quando você deseja disparar várias chamadas de serviço web simultaneamente. Ele também permite que seu servidor web para executar de forma mais eficiente e confiável em condições de carga alta. Um servidor web tem apenas um número limitado de threads disponíveis e, em condições de alta carga quando todos os threads estão em uso, as solicitações de entrada necessário esperar até que threads são liberados. Se o código do aplicativo não lidar com tarefas, como o banco de dados consultas e chamadas de serviço web assíncrona, muitos threads desnecessariamente são vinculados enquanto o servidor está aguardando uma resposta de e/s. Isso limita a quantidade de tráfego, que o servidor pode manipular em condições de carga alta. Com a programação assíncrona, threads que estão aguardando para um serviço web ou banco de dados para retornar dados são liberados para atender novas solicitações até que os dados a é recebida. Em um servidor web ocupado, centenas ou milhares de solicitações podem ser processadas imediatamente que seriam caso contrário, estar aguardando threads a ser liberado.

Como você viu anteriormente, ele é tão fácil diminuir o número de servidores web que seu site da web de tratamento de medida para aumentá-las. Portanto, se um servidor pode obter maior taxa de transferência, você não precisa, muitos deles, e podem reduzir os custos, porque você precisa de menos servidores para um volume de tráfego de determinado que o faria caso contrário.

Suporte para o modelo de programação assíncrono do .NET 4.5 está incluído no ASP.NET 4.5 para Web Forms, MVC e API da Web; no Entity Framework 6 e nos [API do armazenamento do Azure Windows](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Suporte assíncrono no ASP.NET 4.5

No ASP.NET 4.5, o suporte para programação assíncrona tiver sido adicionada, não apenas para o idioma, mas também para as estruturas MVC, Web Forms e Web API. Por exemplo, um método de ação do controlador ASP.NET MVC recebe dados de uma solicitação da web e passa os dados para uma exibição que, em seguida, cria o HTML a serem enviados ao navegador. Com frequência, o método de ação precisa obter dados de um banco de dados ou serviço web para exibi-los em uma página da web ou para salvar os dados inseridos em uma página da web. Nesses cenários, é fácil de usar o método de ação assíncrono: em vez de retornar um *ActionResult* do objeto, você retornar *tarefa&lt;ActionResult&gt;*  e marcar o método com o *async* palavra-chave. Dentro do método, quando uma linha de código inicia uma operação que envolva o tempo de espera, você marcá-la com a palavra-chave await.

Aqui está um método de ação simples que chama um método de repositório para uma consulta de banco de dados:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

E aqui está o mesmo método que manipula a chamada de banco de dados de forma assíncrona:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Nos bastidores, o compilador gera o código assíncrono apropriado. Quando o aplicativo faz a chamada para `FindTaskByIdAsync`, o ASP.NET torna o `FindTask` de solicitação e, em seguida, esvazia o thread de trabalho e o torna disponível para processar outra solicitação. Quando o `FindTask` solicitação é feita, um thread é reiniciado para continuar processando o código que vem após a chamada. Durante o interim entre quando o `FindTask` solicitação é iniciada e quando os dados são retornados, você tem um thread disponível para realizar trabalho útil que, normalmente, seria ser parado aguardando a resposta.

Há alguma sobrecarga para código assíncrono, mas em condições de carga baixa, essa sobrecarga é insignificante, enquanto em condições de carga alta, que você é capaz de processar solicitações que, caso contrário, seriam mantidas aguardando threads disponíveis.

Era possível fazer esse tipo de programação assíncrona desde o ASP.NET 1.1, mas era difícil de escrever, sujeito a erros e difíceis de depurar. Agora que nós Simplificamos a codificação para ele no ASP.NET 4.5, não há nenhum motivo para não fazê-lo mais.

### <a name="async-support-in-entity-framework-6"></a>Suporte assíncrono no Entity Framework 6

Como parte do suporte assíncrono no 4.5, lançamos o suporte assíncrono para chamadas de serviço web, soquetes e sistema de arquivos e/s, mas o padrão mais comum para aplicativos da web é um banco de dados de ocorrências e nossas bibliotecas de dados não oferecia suporte assíncrono. Agora o Entity Framework 6 adiciona suporte assíncrono para acesso ao banco de dados.

No Entity Framework 6, todos os métodos que fazem com que uma consulta ou o comando a ser enviado ao banco de dados tem versões assíncronas. Este exemplo mostra a versão assíncrona do *localizar* método.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

E esse suporte assíncrono funciona não apenas para inserções, exclusões, atualizações e localiza simple, ele também funciona com consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Há um `Async` versão do `ToList` método porque nesse código que é o método que faz com que uma consulta a ser enviado ao banco de dados. O `Where` e `OrderByDescending` métodos apenas configuram a consulta, enquanto a `ToListAsync` método executa a consulta e armazena a resposta no `result` variável.

## <a name="summary"></a>Resumo

Você pode implementar as práticas recomendadas de desenvolvimento de web descritas aqui de qualquer estrutura e em qualquer ambiente de nuvem de programação da web, mas temos que as ferramentas do ASP.NET e do Windows Azure para tornar mais fácil. Se você seguir esses padrões, você pode facilmente escalar horizontalmente sua camada da web, e você vai minimizar as despesas porque cada servidor será capaz de lidar com mais tráfego.

O [próximo capítulo](single-sign-on.md) examina como a nuvem permite cenários de logon únicos.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos.

Servidores web sem monitoração de estado:

- [Microsoft Patterns e práticas recomendadas - diretrizes de dimensionamento automático](https://msdn.microsoft.com/library/dn589774.aspx).
- [Afinidade no Windows Azure Web Sites ARR desabilitação da instância](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Postagem do blog de Erez Benari, explica a afinidade de sessão no Windows Azure Web Sites.

CDN:

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Consulte a discussão de CDN no episódio 3 começando em 1:34:00.
- [Padrão Microsoft Patterns e práticas de hospedagem de conteúdo estático](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revisões CDN](http://www.cdnreviews.com/). Visão geral dos muitos CDNs.

Programação assíncrona:

- [Usar métodos assíncronos no ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programação assíncrona com Async e Await (c# e Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN white paper que explica a lógica para programação assíncrona, como ele funciona no ASP.NET 4.5 e como escrever código para implementá-lo.
- [Consulta do Entity Framework Async e salvar](https://msdn.microsoft.com/data/jj819165)
- [Como criar aplicativos Web do ASP.NET usando o Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Apresentação em vídeo de Rowan Miller. Inclui uma demonstração de gráfico de como a programação assíncrona pode facilitar a enorme aumento na taxa de transferência do web server sob condições de alta carga.
- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Para discussões sobre o impacto da programação assíncrona na escalabilidade, consulte episódio 4 e 8 de episódio.
- [A mágica do uso de métodos assíncronos no ASP.NET 4.5 mais uma pegadinha importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Postagem do blog de Scott Hanselman, principalmente sobre como usar async em aplicativos de Web Forms do ASP.NET.

Para práticas recomendadas de desenvolvimento da web adicionais, consulte os seguintes recursos:

- [A correção do exemplo de aplicativo – práticas recomendadas](the-fix-it-sample-application.md#bestpractices). O Apêndice para este livro eletrônico relaciona um número de práticas recomendadas que foram implementadas no aplicativo Fix It.
- [Lista de verificação de desenvolvedor da Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md)
> [Próximo](single-sign-on.md)
