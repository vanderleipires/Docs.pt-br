---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Padrão centrado em fila (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: f9916e4ecbe6234ee12bcb56519e7e2c0e490972
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840175"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Padrão centrado em fila (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Anteriormente, vimos que usando vários serviços pode resultar em um SLA "composto", em que o SLA de efetiva do aplicativo é o *produto* dos SLAs individuais. Por exemplo, o aplicativo Fix It usa Sites da Web, armazenamento e banco de dados SQL. Se qualquer um desses serviços falhar, o aplicativo retornará um erro ao usuário.

O cache é uma boa maneira de lidar com falhas transitórias para conteúdo somente leitura. Mas e se o seu aplicativo precisa para fazer o trabalho? Por exemplo, quando o usuário envia uma nova Fix It tarefa, o aplicativo não é possível simplesmente colocar a tarefa no cache. O aplicativo precisa para escrever a tarefa corrigi-lo em um armazenamento de dados persistentes, para que ela possa ser processada.

É aí que entra o padrão de trabalho centrado em fila. Esse padrão permite que um acoplamento entre uma camada da web e um serviço de back-end.

Aqui está como funciona o padrão. Quando o aplicativo recebe uma solicitação, ele coloca um item de trabalho em uma fila e retorna imediatamente a resposta. Em seguida, um processo separado de back-end efetua pull de itens de trabalho da fila e faz o trabalho.

O padrão de trabalho centrado em fila é útil para:

- Trabalho que é demorada (alta latência).
- Trabalho que requer um serviço externo que pode não estar sempre disponível.
- O trabalho intensivo de recursos (alta utilização da CPU).
- Trabalho que se beneficiaria da taxa de redistribuição (sujeito a explosões repentinas de carga).

## <a name="reduced-latency"></a>Redução da latência

As filas são úteis sempre que você estiver fazendo tarefas demoradas. Se uma tarefa leva alguns segundos ou mais, em vez de bloquear o usuário final, coloca o item de trabalho em uma fila. Informar ao usuário "Estamos trabalhando nele" e, em seguida, usar um ouvinte de fila para processar a tarefa em segundo plano.

Por exemplo, quando você adquire algo em uma loja online, o site da web confirma seu pedido imediatamente. Mas isso não significa que tudo sobre o aplicativo já está em um caminhão sendo entregues. Eles colocarem uma tarefa em uma fila, e em segundo plano que eles estão fazendo a verificação de crédito, preparando seus itens para envio, em assim por diante.

Para cenários com latência curto, o tempo total de ponta a ponta pode ter mais de uso de uma fila, em comparação com fazendo a tarefa de forma síncrona. Mas mesmo assim, os outros benefícios podem superar essa desvantagem.

## <a name="increased-reliability"></a>Maior confiabilidade

A versão do Fix It que estamos vendo até agora, o front-end da web está acoplado ao back-end do banco de dados SQL. Se o serviço de banco de dados SQL não estiver disponível, o usuário obterá um erro. Se as tentativas não funcionam (ou seja, a falha é mais de transitória), a única coisa que você pode fazer é mostrar um erro e pedir ao usuário que tente novamente mais tarde.

![Front-end com falha quando ocorrer falha de banco de dados SQL back-end da web de mostrando de diagrama](queue-centric-work-pattern/_static/image1.png)

Usando filas, quando um usuário envia uma tarefa para corrigi-lo, o aplicativo grava uma mensagem à fila. A carga da mensagem é uma [JSON](http://json.org/) representação da tarefa. Assim que a mensagem é gravada na fila, o aplicativo retorna e mostra imediatamente uma mensagem de êxito para o usuário.

Se qualquer um dos serviços de back-end – como o banco de dados SQL ou o ouvinte de fila – fique offline, os usuários ainda podem enviar novas Fix It tarefas. As mensagens simplesmente colocará em fila até que os serviços de back-end estão disponíveis novamente. Nesse ponto, os serviços de back-end serão atualizada na lista de pendências.

![Diagrama mostrando a web front-end continuar a funcionar quando há um erro de banco de dados SQL](queue-centric-work-pattern/_static/image2.png)

Além disso, agora você pode adicionar mais lógica de back-end sem se preocupar sobre a resiliência do front-end. Por exemplo, você talvez queira enviar um email ou mensagem SMS para o proprietário, sempre que um novo Fix It é atribuído. Se o email ou o serviço SMS ficar indisponível, você pode processar tudo e, em seguida, colocar uma mensagem em uma fila separada para enviar mensagens de email/SMS.

Anteriormente, nosso SLA efetivo era aplicativos Web &times; armazenamento &times; banco de dados SQL = 99,7%. (Consulte [Design para resistir a falhas](design-to-survive-failures.md).)

Ao alterar o aplicativo use uma fila, o front-end da web depende apenas aplicativos Web e de armazenamento, de uma composição de SLA de 99,8%. (Observe que as filas são parte do serviço de armazenamento do Azure, para que sejam incluídos no SLA mesmo como o armazenamento de BLOBs).

Se você precisar ainda melhor 99,8%, você pode criar duas filas em duas regiões diferentes. Designe um como primário e o outro como secundário. Em seu aplicativo, fazer failover para a fila secundária se a fila primária não estiver disponível. A possibilidade dos dois estarem indisponíveis ao mesmo tempo é muito pequena.

## <a name="rate-leveling-and-independent-scaling"></a>Redistribuição de taxa e dimensionamento independentes

As filas também são úteis para algo chamado *nivelamento de taxa* ou *nivelamento de carga*.

Aplicativos Web geralmente são suscetíveis a picos repentinos no tráfego. Embora você possa usar o dimensionamento automático para adicionar automaticamente os servidores web para manipular o tráfego da web maior, dimensionamento automático não poderá reagir rápido o suficiente para lidar com picos abruptas de carga. Se os servidores web podem descarregar parte do trabalho que eles precisam fazer gravando uma mensagem para uma fila, eles podem lidar com mais tráfego. Um serviço de back-end, em seguida, pode ler as mensagens da fila e processá-las. A profundidade da fila será ampliada ou reduzida conforme a carga de entradavariar.

Grande parte do seu trabalho demorado ser carregado para um serviço de back-end, a camada da web mais facilmente pode responder a picos repentinos no tráfego. E economize dinheiro porque qualquer determinada quantidade de tráfego pode ser tratada pelo menos servidores web.

Você pode dimensionar a camada da web e o serviço de back-end independentemente. Por exemplo, talvez seja necessário três servidores web, mas apenas um mensagens de fila de processamento de servidor. Ou, se você estiver executando uma tarefa de computação intensa em segundo plano, talvez seja necessário mais servidores de back-end.

![](queue-centric-work-pattern/_static/image3.png)

Dimensionamento automático funciona com os serviços de back-end, bem como com a camada da web. Você pode escalar verticalmente ou reduzir verticalmente o número de VMs que estão sendo processadas as tarefas na fila, com base no uso da CPU de VMs de back-end. Ou, você pode ser dimensionado automaticamente com base em quantos itens estão em uma fila. Por exemplo, você pode dizer a AutoEscala para tentar manter não mais de 10 itens na fila. Se a fila tiver mais de 10 itens, o dimensionamento automático adicionará as VMs. Quando forem recuperados, o dimensionamento automático será subdividir as VMs adicionais.

## <a name="adding-queues-to-the-fix-it-application"></a>Adicionando a enfileira para a correção de aplicativo

Para implementar o padrão de fila, precisamos fazer duas alterações no aplicativo corrigi-lo.

- Quando um usuário envia uma nova Fix It tarefa, coloque a tarefa na fila, em vez de gravá-los no banco de dados.
- Crie um serviço de back-end que processa mensagens na fila.

Para a fila, vamos usar o [serviço de armazenamento de fila do Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Outra opção é usar [do barramento de serviço do Azure](https://docs.microsoft.com/azure/service-bus/).

Para decidir qual serviço de fila usar, considere como o seu aplicativo precisa para enviar e receber as mensagens na fila:

- Se você tiver a cooperação produtores e consumidores concorrentes, considere usar o serviço de armazenamento de fila do Azure. "Cooperativas produtores" significa que vários processos são adicionar mensagens a uma fila. "Consumidores concorrentes" significa que vários processos estão recebendo mensagens da fila para processá-las, mas qualquer mensagem determinada só pode ser processada por um "consumidor". Se você precisar de mais taxa de transferência que você pode começar com uma única fila, use filas adicionais e/ou contas de armazenamento adicionais.
- Se você precisar de uma [modelo de publicação/assinatura](http://en.wikipedia.org/wiki/Publish/subscribe), considere o uso de filas do barramento de serviço do Azure.

O aplicativo Fix It ajusta os produtores de cooperação e o modelo de consumidores concorrentes.

Outra consideração é a disponibilidade do aplicativo. O serviço de armazenamento de fila é parte do mesmo serviço que estamos usando para o armazenamento de blob, portanto, usá-lo não tem efeito sobre nosso SLA. O barramento de serviço do Azure é um serviço separado com seu próprio SLA. Se usarmos filas do barramento de serviço, teríamos que considerar um percentual de SLA adicional e nosso SLA composto seria menor. Quando você está escolhendo um serviço de fila, certifique-se de que entender o impacto de sua escolha na disponibilidade do aplicativo. Para obter mais informações, consulte o [recursos](#resources) seção.

## <a name="creating-queue-messages"></a>Criando fila de mensagens

Para colocar uma tarefa Fix It na fila, o front-end da web executa as seguintes etapas:

1. Criar uma [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instância. O `CloudQueueClient` instância é usada para executar solicitações no serviço fila.
2. Crie a fila, caso ele ainda não exista.
3. Serialize a tarefa corrigir.
4. Chame [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar a mensagem na fila.

Vamos fazer esse trabalho no construtor e `SendMessageAsync` método de um novo `FixItQueueManager` classe.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aqui, estamos usando o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para serializar o fixit em formato JSON. Você pode usar a abordagem de serialização que preferir. O JSON tem a vantagem de ser legíveis, embora seja menos detalhada do que o XML.

Código de qualidade de produção seria adicionar lógica de tratamento de erros, pausar se o banco de dados ficou indisponível, lidar com a recuperação mais corretamente, criar a fila na inicialização do aplicativo e gerenciar "[suspeitas" mensagens](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Uma mensagem suspeita é uma mensagem que não pode ser processada por alguma razão. Você não quer mensagens suspeitas para ficar na fila, em que a função de trabalho continuamente tentará processá-las, falhar, tente novamente, falha e assim por diante.)

O aplicativo front-end do MVC, precisamos atualizar o código que cria uma nova tarefa. Em vez de colocar a tarefa para o repositório, chame o `SendMessageAsync` método mostrado acima.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Processando mensagens de fila

Para processar mensagens na fila, vamos criar um serviço de back-end. O serviço de back-end será executado um loop infinito que executa as seguintes etapas:

1. Obter a próxima mensagem da fila.
2. Desserialize a mensagem a uma tarefa para corrigi-lo.
3. Gravar a tarefa corrigir o banco de dados.

Para hospedar o serviço de back-end, vamos criar um serviço de nuvem do Azure que contém um *função de trabalho*. Uma função de trabalho consiste em uma ou mais VMs que podem fazer o processamento de back-end. O código executado nessas VMs irão receber mensagens da fila conforme eles ficam disponíveis. Para cada mensagem, vamos desserializar o conteúdo JSON e escrever uma instância da entidade corrigir sua tarefa no banco de dados, usando o mesmo repositório que usamos anteriormente na camada da web.

As etapas a seguir mostram como adicionar um trabalho do projeto de função a uma solução que tem um projeto da web padrão. Essas etapas já tem sido feitas no Fix It projeto que você pode baixar.

Primeiro, adicione um projeto de serviço de nuvem à solução do Visual Studio. A solução com o botão direito e selecione **Add**, em seguida, **novo projeto**. No painel esquerdo, expanda **Visual c#** e selecione **nuvem**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

No **novo serviço de nuvem do Azure** caixa de diálogo, expanda o **Visual c#** nó no painel esquerdo. Selecione **função de trabalho** e clique no ícone de seta para a direita.

![](queue-centric-work-pattern/_static/image6.png)

(Observe que você também pode adicionar um *função web*. Podemos pode executar o Fix It front-end no mesmo serviço de nuvem, em vez de executá-lo em um Site da Web do Azure. Que tem algumas vantagens em facilitar as conexões entre o front-end e back-end coordenar. No entanto, para simplificar esta demonstração, estamos mantendo o front-end em um aplicativo de Web do serviço de aplicativo do Azure e apenas executando o back-end em um serviço de nuvem.)

Um nome padrão é atribuído à função de trabalho. Para alterar o nome, passe o mouse sobre a função de trabalho no painel direito e, em seguida, clique no ícone de lápis.

![](queue-centric-work-pattern/_static/image7.png)

Clique em **Okey** para concluir a caixa de diálogo. Isso adiciona dois projetos à solução do Visual Studio.

- um projeto do Azure que define o serviço de nuvem, incluindo informações de configuração.
- Um projeto de função de trabalho que define a função de trabalho.

![](queue-centric-work-pattern/_static/image8.png)

Para obter mais informações, consulte [criando um projeto do Azure com o Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dentro da função de trabalho, podemos sondar mensagens chamando o `ProcessMessageAsync` método da `FixItQueueManager` classe que vimos anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

O `ProcessMessagesAsync` método verifica se há uma espera de mensagem. Se houver um, ele desserializa a mensagem em uma `FixItTask` entidade e salva a entidade no banco de dados. Ele executa um loop até que a fila está vazia.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondagem da fila de mensagens resulta em uma transação pequeno cobrar, portanto, quando nenhuma mensagem aguardando para serem processados, a função de trabalho `RunAsync` método aguarda por um segundo antes de sondar novamente chamando `Task.Delay(1000)`.

Em um projeto web, adicionar código assíncrono pode automaticamente melhorar o desempenho porque o IIS gerencia um pool de threads limitado. Esse não for o caso em um projeto de função de trabalho. Para melhorar a escalabilidade da função de trabalho, você pode escrever códigos com multithread ou usar o código assíncrono para implementar [programação paralela](https://msdn.microsoft.com/library/ff963553.aspx). O exemplo não implementa a programação paralela, mas mostra como tornar o código assíncrono, para que você possa implementar a programação paralela.

## <a name="summary"></a>Resumo

Neste capítulo, você viu como melhorar a escalabilidade, confiabilidade e capacidade de resposta do aplicativo, Implementando o padrão de trabalho centrado em fila.

Este é o último dos padrões de 13 abordados neste livro eletrônico, mas certamente há muitos outros padrões e práticas recomendadas que podem ajudar você a criar aplicativos de nuvem bem-sucedida. O [capítulo final](more-patterns-and-guidance.md) fornece links para recursos de tópicos que ainda não foram abordados nesses padrões 13.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações sobre filas, consulte os seguintes recursos.

Documentação:

- [Parte de filas de armazenamento do Microsoft Azure 1: Introdução ao](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artigo de Roman Schacherl.
- [Executando tarefas em segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), capítulo 5 [movendo aplicativos para a nuvem, 3ª edição](https://msdn.microsoft.com/library/ff728592.aspx) da Microsoft Patterns and Practices. (Em particular, a seção ["Usando filas do armazenamento do Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Práticas recomendadas para maximizar a escalabilidade e a relação custo benefício de soluções de mensagens baseadas em fila no Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White paper, Valery Mizonov.
- [Comparando as filas do Azure e filas do barramento de serviço](https://msdn.microsoft.com/magazine/jj159884.aspx). Artigo da MSDN Magazine, fornece informações adicionais que podem ajudar você a escolha de qual serviço de fila para usar. O artigo menciona que o barramento de serviço é dependente de ACS para autenticação, o que significa que suas filas SB estariam disponíveis ao ACS não está disponível. No entanto, uma vez que o artigo foi escrito, SB foi alterado para que você possa usar [tokens SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como uma alternativa ao ACS.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte primer de mensagens assíncrono, o padrão de Pipes e filtros, padrão de transação de compensação, padrão de consumidores concorrentes, padrão CQRS.
- [CQRS Journey](https://msdn.microsoft.com/library/jj554200). Livro eletrônico sobre o CQRS por padrões e práticas Microsoft.

Vídeo:

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Para obter uma introdução ao serviço de armazenamento do Azure e filas, consulte episódio 5 começando em 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Próximo](more-patterns-and-guidance.md)
