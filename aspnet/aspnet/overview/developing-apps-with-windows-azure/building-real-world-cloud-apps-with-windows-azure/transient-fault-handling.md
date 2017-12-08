---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: "Transient Fault Handling (compilação de aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 3caeeb83e4c074ae0ffc30f035d793a821eb6be2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (compilação de aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Quando você estiver criando um aplicativo de nuvem do mundo real, uma das coisas que você precisa pensar é como lidar com interrupções de serviço temporária. Esse problema é exclusivamente importante em aplicativos de nuvem porque está tão dependente conexões de rede e serviços externos. Você pode obter com frequência pequenos problemas que normalmente são autorrecuperação e se você não estiver preparado para tratá-los de forma inteligente, serão resultar em uma experiência desagradável para seus clientes.

## <a name="causes-of-transient-failures"></a>Causas de falhas transitórias

No ambiente de nuvem, você encontrará que falharam e quedas de conexões de banco de dados ocorrer periodicamente. Isso ocorre em parte porque você usará o mais balanceadores de carga em comparação comparadas o ambiente local em que o servidor web e o servidor de banco de dados tem uma conexão física direta. Além disso, às vezes, quando você estiver depende de um serviço multilocatário verá chamadas para o serviço get mais lento ou tempo limite porque outra pessoa que usa o serviço está atingindo muito. Em outros casos, você pode ser o usuário que está acessando o serviço com muita frequência e o serviço limita você – nega conexões – deliberadamente para impedir a afetar outros locatários do serviço.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Use a lógica de repetição/retirada inteligente para reduzir o efeito de falhas transitórias

Em vez de gerar uma exceção e exibir uma página de erro ou não está disponível para o cliente, você pode reconhecer os erros que são normalmente transitórios e automaticamente de repetir a operação que resultou em erro, em espera que antes de tempo será bem-sucedida. Na maioria das vezes, a operação terá êxito na segunda tentativa, e você vai recuperar do erro sem o cliente já ter sido ciente de que houve um problema.

Há várias maneiras que você pode implementar a lógica de repetição inteligentes.

- A Microsoft Patterns &amp; práticas grupo tem um [Transient Fault Handling Application Block](https://msdn.microsoft.com/en-us/library/dn440719(v=pandp.60).aspx) que faz tudo para você se você estiver usando o ADO.NET para acesso ao banco de dados SQL (não por meio do Entity Framework). Você apenas definir uma política para repetições – quantas vezes para tentar novamente uma consulta ou comando e por quanto tempo a aguardar entre tentativas – e wrap SQL código em um *usando* bloco.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Também suporta TFH [Cache na função do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/dn386103.aspx) e [barramento de serviço](https://azure.microsoft.com/services/service-bus/).
- Quando você usa o Entity Framework você normalmente não está trabalhando diretamente com conexões de SQL, para que você não pode usar este pacote de padrões e práticas recomendadas, mas o Entity Framework 6 cria esse tipo de lógica de repetição à direita na estrutura de. Da mesma forma que você especificar a estratégia de repetição e, em seguida, EF usa essa estratégia sempre que ele acessa o banco de dados.

    Para usar esse recurso no aplicativo corrigir, tudo o que precisamos fazer é adicionar uma classe que deriva de *DbConfiguration* e ativar a lógica de repetição.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Exceções de banco de dados SQL que identifica a estrutura como erros transitórios normalmente, o código mostrado instrui o EF para repetir a operação até 3 vezes, com um atraso de retirada exponencial entre as repetições e um atraso máximo de 5 segundos. Retirada exponencial significa que após cada tentativa com falha-irá aguardar um período mais longo de tempo antes de tentar novamente. Se falharem em três tentativas em uma linha, ela irá gerar uma exceção. A seção a seguir sobre disjuntores explica por que você deseja retirada exponencial e um número limitado de novas tentativas.

    Quando você estiver usando o serviço de armazenamento do Azure, como o aplicativo corrigir não para Blobs, e a API de cliente de armazenamento .NET já implementa o mesmo tipo de lógica, você pode ter problemas semelhantes. Você simplesmente especifica a política de repetição ou até mesmo não é necessário fazer isso se você estiver satisfeito com as configurações padrão.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disjuntores

Há várias razões por que você não deseja tentar novamente muitas vezes em um período muito longo:

- Muitos usuários de maneira persistente repetir solicitações com falha podem afetar a experiência de outros usuários. Se milhões de pessoas estão todos fazendo repetidas repetir solicitações pode ser associando as filas de expedição do IIS e impedindo que o seu aplicativo atendendo a solicitações que ele pode manipular com êxito.
- Se todos está repetindo devido a uma falha de serviço, há podem muitas solicitações em fila que o serviço obtém inundado quando começar a recuperar.
- Se o erro for devido à limitação e há uma janela de tempo que o serviço usa para limitação, repetições contínuas foi possível mover a janela a e fazer com que a limitação continuar.
- Você pode ter um usuário que está aguardando uma página da web para processar. Espera de pessoas fazer muito talvez mais irritantes que relativamente rapidamente informando-os e tente novamente mais tarde.

Retirada exponencial aborda alguns desse problema a limitar a frequência de repetições que pode obter um serviço de seu aplicativo. Mas você também precisará ter *disjuntores*: isso significa que, em um determinado Repita limite seu aplicativo para tentar novamente e executa outra ação, como um dos seguintes:

- Fallback personalizado. Se você não pode obter um preço de estoque de Reuters, talvez você pode obtê-lo de Bloomberg; ou, se você não pode obter dados do banco de dados, talvez você pode obtê-lo do cache.
- Falha silenciosa. Se você precisa de um serviço não for tudo ou nada para seu aplicativo, basta retorne nulo quando não é possível obter os dados. Se você estiver exibindo uma tarefa corrigir e o serviço Blob não está respondendo, você pode exibir os detalhes da tarefa sem a imagem.
- Falhe no modo rápido. Erro do usuário para evitar a saturação do serviço com Repita solicitações que podem causar a interrupção do serviço para outros usuários ou estender o período de limitação. Você pode exibir uma mensagem amigável "tente novamente mais tarde".

Não há nenhuma política de repetição se aplicam. Você pode repetir mais vezes e esperar mais em um processo de trabalho assíncrono em segundo plano que você faria em um aplicativo web síncrono em que um usuário está aguardando uma resposta. Você pode esperar mais entre as tentativas de um serviço de banco de dados relacional que você usaria para um serviço de cache. Aqui estão alguns exemplo recomendado Repita políticas para lhe dar uma ideia de como os números podem variar. (Nenhum atraso antes que a primeira nova tentativa significa "Rápido primeiro".

![Políticas de repetição de exemplo](transient-fault-handling/_static/image1.png)

Para orientação de política de repetição de banco de dados SQL, consulte [solucionar erros de conexão para o banco de dados SQL e falhas transitórias](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumo

Uma estratégia de repetição/retirada pode ajudar a tornar erros temporários invisível para o cliente na maioria das vezes, e a Microsoft fornece estruturas que você pode usar para minimizar o trabalho de implementar uma estratégia se você estiver usando o ADO.NET, Entity Framework ou o Azure Serviço de armazenamento.

No [próximo capítulo](distributed-caching.md), vamos examinar como melhorar o desempenho e confiabilidade usando cache distribuído.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

Documentação

- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy. Semelhante ao entrar em mais detalhes instruções mas série à prova de falhas. Consulte a seção de telemetria e diagnóstico.
- [À prova de falhas: Orientação para arquiteturas resilientes na nuvem](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versão de página da Web da série de vídeo à prova de falhas.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Consulte repetição padrão, o padrão do Supervisor de agente do Agendador.
- [Tolerância a falhas no banco de dados SQL do Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Postagem no blog por Tony Petrossian.
- [Entity Framework - resiliência de Conexão / lógica de repetição](https://msdn.microsoft.com/en-us/data/dn456835). Como usar e personalizar o transient fault handling recurso do Entity Framework 6.
- [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Quarta etapa em uma série de tutoriais de nove partes, mostra como configurar o recurso de resiliência de conexão EF 6 para o banco de dados SQL.

Vídeos

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Consulte a discussão sobre disjuntores no episódio 3 começando em 40:55.
- [Criando Big: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre a criação de falha, transitória de falhas tratamento e instrumentação tudo.

Exemplo de código

- [Conceitos fundamentais do serviço no Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemplo de aplicativo criado pelo Microsoft Azure Customer Advisory Team que demonstra como usar o [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Para obter mais informações, consulte [nuvem conceitos básicos de dados acesso camada de serviço – tratamento de falhas transitórias](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH é recomendada para acesso de banco de dados usando o ADO.NET diretamente (sem usar o Entity Framework).

>[!div class="step-by-step"]
[Anterior](monitoring-and-telemetry.md)
[Próximo](distributed-caching.md)
