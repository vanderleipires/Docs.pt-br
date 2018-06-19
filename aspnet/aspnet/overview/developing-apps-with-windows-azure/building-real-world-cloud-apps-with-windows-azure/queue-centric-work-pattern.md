---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Padrão de trabalho centrado em fila (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 124e673206ecea2eac5efb8c2802a32a690fa104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875429"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Padrão de trabalho centrado em fila (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Versões anteriores, vimos que usar vários serviços pode resultar em um SLA "composto", onde o SLA efetiva do aplicativo é o *produto* de SLAs individuais. Por exemplo, o aplicativo corrigir usa Sites da Web, o armazenamento e o banco de dados SQL. Se qualquer um desses serviços falhar, o aplicativo retornará um erro para o usuário.

O cache é uma boa maneira de lidar com falhas transitórias para conteúdo somente leitura. Mas e se o seu aplicativo precisa trabalhar? Por exemplo, quando o usuário envia uma nova corrigir tarefa, o aplicativo não pode simplesmente coloque a tarefa no cache. O aplicativo precisa gravar a tarefa corrigir em um repositório de dados persistentes, para que possa ser processado.

É aqui que entra o padrão de trabalho centrado em fila. Esse padrão permite que um acoplamento flexível entre uma camada da web e um serviço de back-end.

Aqui está como funciona o padrão. Quando o aplicativo recebe uma solicitação, ele coloca um item de trabalho em uma fila e retorna imediatamente a resposta. Em seguida, um processo de back-end separados extrai itens de trabalho da fila e faz o trabalho.

O padrão de trabalho centrado em fila é útil para:

- Trabalho que é demorada (alta latência).
- Trabalho que exija um serviço externo que pode não estar sempre disponível.
- O trabalho com muitos recursos (CPU alta).
- Trabalho que se beneficiaria de taxa de redistribuição (sujeito a explosões repentinas de carga).

## <a name="reduced-latency"></a>Redução da latência

Filas são úteis sempre que você está realizando tarefas demoradas. Se uma tarefa leva alguns segundos ou mais, em vez de bloquear o usuário final, coloque o item de trabalho em uma fila. Informe o usuário "Estamos trabalhando nele" e, em seguida, usar um ouvinte de fila para processar a tarefa em segundo plano.

Por exemplo, quando você compra algo em uma loja online, o site da web confirma seu pedido imediatamente. Mas isso não significa que tudo sobre o aplicativo já está em um caminhão de entrega. Eles coloque a tarefa em uma fila, e em segundo plano estiver executando a verificação de crédito, Preparando os itens para envio, em assim por diante.

Para cenários com latência curto, o tempo total de ponta a ponta pode ser mais usando uma fila, em comparação com fazendo a tarefa de forma síncrona. Mas, mesmo assim, os outros benefícios podem exceder esse desvantagem.

## <a name="increased-reliability"></a>Maior confiabilidade

Na versão de correção que estamos vendo até agora, o front-end da web está acoplado ao back-end do banco de dados SQL. Se o serviço de banco de dados SQL não estiver disponível, o usuário obterá um erro. Se tentativas não funcionam (ou seja, a falha é mais de transitória), a única coisa que você pode fazer é mostrar um erro e peça que ele tente novamente mais tarde.

![Front-end com falha quando o banco de dados SQL back-end falha da web de exibição de diagrama](queue-centric-work-pattern/_static/image1.png)

Uso de filas, quando um usuário envia uma tarefa corrigir, o aplicativo grava uma mensagem à fila. A carga da mensagem é um [JSON](http://json.org/) representação da tarefa. Assim que a mensagem é gravada na fila, o aplicativo retorna e mostra imediatamente uma mensagem de êxito para o usuário.

Se qualquer um dos serviços de back-end – como o banco de dados SQL ou o ouvinte de fila-- ficar offline, os usuários ainda podem enviar novas corrigir tarefas. As mensagens ficarão apenas na fila até que os serviços de back-end estão disponíveis novamente. Nesse ponto, os serviços de back-end serão atualizado na lista de pendências.

![Diagrama mostrando web front-end continuar a funcionar quando há um erro de banco de dados SQL](queue-centric-work-pattern/_static/image2.png)

Além disso, agora você pode adicionar mais lógica de back-end sem se preocupar com a resiliência do front-end. Por exemplo, você talvez queira enviar um email ou mensagem SMS para o proprietário, sempre que um novo corrigir é atribuído. Se o email ou o serviço SMS ficar indisponível, você pode processar tudo e, em seguida, colocar uma mensagem em uma fila separada para enviar mensagens de email para o SMS.

Anteriormente, nosso SLA efetivo era aplicativos Web &times; armazenamento &times; banco de dados SQL = 99,7%. (Consulte [Design sobreviver a falhas](design-to-survive-failures.md).)

Ao alterar o aplicativo para usar uma fila, o front-end da web depende apenas em aplicativos Web e de armazenamento, para um SLA de 99,8% composto. (Observe que as filas são parte do serviço de armazenamento do Azure, para que sejam incluídos no SLA mesmo como o armazenamento de blob).

Se você precisar ainda melhor 99,8%, você pode criar duas filas em duas regiões diferentes. Designe um como primário e o outro como secundário. Em seu aplicativo, fazer failover para a fila secundária se a fila principal não está disponível. A possibilidade de ambos os indisponibilidade ao mesmo tempo é muito pequena.

## <a name="rate-leveling-and-independent-scaling"></a>Taxa de redistribuição e dimensionamento independente

As filas também são úteis para algo chamado *taxa nivelamento* ou *nivelamento de carga*.

Aplicativos Web geralmente são suscetíveis a picos repentinos de tráfego. Embora você possa usar o dimensionamento automático para adicionar automaticamente os servidores web para lidar com o tráfego da web maior, dimensionamento automático não poderá reagir rápido o suficiente para lidar com picos abruptas na carga. Se os servidores web podem descarregar uma parte do trabalho, precisarão fazê-lo ao escrever uma mensagem para uma fila, eles possam lidar com mais tráfego. Um serviço de back-end pode ler mensagens da fila e processá-las. A profundidade da fila será ampliada ou reduzida conforme a carga de entrada varia.

Com muito de seu trabalho demorado tiver sido descarregado em um serviço de back-end, a camada da web mais facilmente pode responder a picos repentinos na tráfego. E economizar dinheiro porque qualquer determinada quantidade de tráfego pode ser tratada pelo menos servidores web.

Você pode dimensionar a camada da web e o serviço de back-end independentemente. Por exemplo, talvez seja necessário três servidores de web mas mensagens apenas um da fila de processamento do servidor. Ou, se você estiver executando uma tarefa com uso intensivo de computação em segundo plano, você talvez precise de mais servidores de back-end.

![](queue-centric-work-pattern/_static/image3.png)

Dimensionamento automático funciona com os serviços de back-end, bem como a camada da web. Você pode expandir ou reduzir o número de VMs que estão sendo processadas as tarefas na fila, com base no uso da CPU de back-end VMs. Ou, você pode fazer AutoEscala com base em quantos itens estão em uma fila. Por exemplo, você pode informar ao tentar manter não mais do que 10 itens na fila de dimensionamento automático. Se a fila tem mais de 10 itens, o dimensionamento automático adicionará VMs. Quando eles ficasse, AutoEscala desativa as VMs adicionais.

## <a name="adding-queues-to-the-fix-it-application"></a>Adicionando enfileira a correção de aplicativo

Para implementar o padrão de fila, precisamos fazer duas alterações para o aplicativo para corrigi-lo.

- Quando um usuário envia uma nova corrigir tarefa, coloque a tarefa em fila, em vez de gravação no banco de dados.
- Crie um serviço de back-end que processa as mensagens na fila.

Para a fila, usaremos o [serviço de armazenamento de fila do Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Outra opção é usar [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Para decidir qual serviço de fila usar, considere como o seu aplicativo precisa para enviar e receber as mensagens na fila:

- Se você tiver cooperação produtores e consumidores concorrentes, considere usar o serviço de armazenamento de fila do Azure. "Trabalhar de forma produtores" significa que vários processos estão adicionar mensagens a uma fila. "Consumidores concorrentes" significa que vários processos estão recebendo mensagens da fila para processá-las, mas qualquer determinada mensagem só pode ser processada por um "cliente". Se você precisar de mais taxa de transferência que você pode obter com uma única fila, use filas adicionais e/ou contas de armazenamento adicionais.
- Se você precisar de um [modelo de publicação/assinatura](http://en.wikipedia.org/wiki/Publish/subscribe), considere o uso de filas do barramento de serviço do Azure.

O aplicativo corrigir se ajusta a cooperação produtores e o modelo de consumidores concorrentes.

Outra consideração é a disponibilidade de aplicativos. O serviço de armazenamento de fila é parte do mesmo serviço que estamos usando para o armazenamento de blob, para que usá-lo não tem nenhum efeito em nosso SLA. Barramento de serviço do Azure é um serviço separado com seu próprio SLA. Se usamos filas do barramento de serviço, seria preciso considerar uma porcentagem de SLA adicional e nosso SLA composto seria inferior. Quando você escolhe um serviço de fila, certifique-se de que entender o impacto de sua escolha na disponibilidade do aplicativo. Para obter mais informações, consulte o [recursos](#resources) seção.

## <a name="creating-queue-messages"></a>Criar fila de mensagens

Para colocar uma tarefa corrigir na fila, o front-end da web executa as seguintes etapas:

1. Criar um [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instância. O `CloudQueueClient` instância é usada para executar solicitações no serviço de fila.
2. Crie a fila, se ele ainda não existir.
3. Serialize a tarefa corrigir.
4. Chamar [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar a mensagem na fila.

Vamos fazer esse trabalho no construtor e `SendMessageAsync` método de um novo `FixItQueueManager` classe.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aqui, estamos usando o [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para serializar fixit em formato JSON. Você pode usar qualquer abordagem de serialização que preferir. JSON tem a vantagem de ser legível, enquanto estiver sendo menos detalhada do que o XML.

Código de qualidade de produção seria adicionar lógica de tratamento de erros, pausar se o banco de dados ficou indisponível, lidar com recuperação mais corretamente, criar a fila na inicialização do aplicativo e gerenciar "[suspeita" mensagens](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Uma mensagem suspeita é uma mensagem que não pode ser processada por alguma razão. Você não quer mensagens suspeitas para ficar na fila, em que a função de trabalho continuamente tentará processá-las, falhar, tente novamente, falha e assim por diante.)

O aplicativo front-end do MVC, precisamos atualizar o código que cria uma nova tarefa. Em vez de colocar a tarefa para o repositório, chame o `SendMessageAsync` método mostrado acima.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Processando mensagens de fila

Para processar mensagens na fila, vamos criar um serviço de back-end. O serviço de back-end será executado um loop infinito que executa as seguintes etapas:

1. Obtenha a próxima mensagem da fila.
2. Desserialize a mensagem para uma tarefa corrigir.
3. Grave a tarefa corrigir o banco de dados.

Para hospedar o serviço de back-end, vamos criar um serviço de nuvem do Azure que contém um *função de trabalho*. Uma função de trabalho consiste em uma ou mais VMs que podem fazer o processamento de back-end. O código que executa essas VMs efetuarão pull das mensagens da fila assim que estiverem disponíveis. Para cada mensagem, vamos desserializar a carga de JSON e escrever uma instância da entidade corrigir sua tarefa no banco de dados, usando o mesmo repositório que usamos anteriormente na camada da web.

As etapas a seguir mostram como adicionar um trabalhador projeto de função para uma solução que tem um projeto da web padrão. Essas etapas já tenham sido feitas no corrigir projeto que você pode baixar.

Primeiro, adicione um projeto de serviço de nuvem à solução do Visual Studio. A solução e selecione **adicionar**, em seguida, **novo projeto**. No painel esquerdo, expanda **Visual C#** e selecione **nuvem**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

No **novo serviço de nuvem do Azure** caixa de diálogo, expanda o **Visual C#** nó no painel esquerdo. Selecione **função de trabalho** e clique no ícone de seta para a direita.

![](queue-centric-work-pattern/_static/image6.png)

(Observe que você também pode adicionar uma *função web*. Podemos pode executar o Fix It front-end no mesmo serviço de nuvem em vez de executá-lo em um Site do Azure. Que tem algumas vantagens em facilitar as conexões entre o front-end e back-end coordenar. No entanto, para manter esta demonstração simples, estamos mantendo o front-end em um aplicativo de Web do serviço de aplicativo do Azure e executar apenas o back-end em um serviço de nuvem.)

Um nome padrão é atribuído à função de trabalho. Para alterar o nome, passe o mouse sobre a função de trabalho no painel direito, em seguida, clique no ícone de lápis.

![](queue-centric-work-pattern/_static/image7.png)

Clique em **Okey** para concluir a caixa de diálogo. Isso adiciona dois projetos à solução do Visual Studio.

- um projeto do Azure que define o serviço de nuvem, incluindo informações de configuração.
- Um projeto de função de trabalho que define a função de trabalho.

![](queue-centric-work-pattern/_static/image8.png)

Para obter mais informações, consulte [criando um projeto do Azure com o Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dentro da função de trabalho, podemos pesquisar mensagens chamando o `ProcessMessageAsync` método o `FixItQueueManager` classe que vimos anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

O `ProcessMessagesAsync` método verifica se há uma espera de mensagem. Se houver, ele desserializa a mensagem em uma `FixItTask` entidade e salva a entidade no banco de dados. Ele será repetido até que a fila está vazia.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondagem de fila de mensagens incorre em uma transação de pequena cobram, portanto, quando nenhuma mensagem aguardando para serem processados, a função de trabalho `RunAsync` método espera um segundo antes de sondar novamente chamando `Task.Delay(1000)`.

Em um projeto da web, adicionar o código assíncrono pode automaticamente melhorar o desempenho porque o IIS gerencia um pool de threads limitado. Esse não for o caso em um projeto de função de trabalho. Para melhorar a escalabilidade da função de trabalho, você pode escrever código ou usar código assíncrono para implementar [programação paralela](https://msdn.microsoft.com/library/ff963553.aspx). O exemplo não implementa a programação paralela, mas mostra como fazer o código assíncrono para que você possa implementar programação paralela.

## <a name="summary"></a>Resumo

Neste capítulo, você viu como melhorar a escalabilidade, confiabilidade e capacidade de resposta do aplicativo Implementando o padrão de trabalho centrado em fila.

Esta é a última de 13 padrões abordados neste livro eletrônico, mas é claro, há muitos outros padrões e práticas que podem ajudá-lo a criar aplicativos de nuvem com êxito. O [capítulo final](more-patterns-and-guidance.md) fornece links para recursos de tópicos que ainda não foram abordados nesses 13 padrões.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações sobre as filas, consulte os seguintes recursos.

Documentação:

- [Parte de filas de armazenamento do Microsoft Azure 1: Guia de Introdução](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artigo de Roman Schacherl.
- [Executar tarefas em segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), capítulo 5 do [movendo aplicativos para a nuvem, 3ª edição](https://msdn.microsoft.com/library/ff728592.aspx) Microsoft Patterns e práticas recomendadas. (Em particular, a seção ["Usando filas de armazenamento do Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Práticas recomendadas para maximizar a escalabilidade e a relação custo-benefício de soluções de mensagens baseadas em fila no Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White paper, Valery Mizonov.
- [Comparando as filas do Azure e filas do barramento de serviço](https://msdn.microsoft.com/magazine/jj159884.aspx). Artigo de revista MSDN fornece informações adicionais que podem ajudá-lo a escolher qual serviço de fila a ser usado. O artigo menciona que o barramento de serviço é dependente de ACS para autenticação, o que significa que as filas do SB seria indisponíveis quando o ACS não está disponível. No entanto, desde que o artigo foi escrito, SB foi alterada para que você possa usar [tokens SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como uma alternativa ao ACS.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/library/dn568099.aspx). Consulte primer mensagens assíncronas, Pipes e filtros padrão, o padrão de transações de compensação, padrão de consumidores concorrentes, padrão CQRS.
- [Viagem de CQRS](https://msdn.microsoft.com/library/jj554200). Livro eletrônico sobre CQRS pelo Microsoft padrões e práticas recomendadas.

Vídeo:

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Para obter uma introdução para o serviço de armazenamento do Azure e filas, consulte episódio 5 começando em 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Próximo](more-patterns-and-guidance.md)
