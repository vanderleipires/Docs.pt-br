---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Transient Fault Handling (criação de aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 13ed8c2373c22070d21519bc495161e956b0ac4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398408"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Transient Fault Handling (criação de aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Quando você estiver criando um aplicativo de nuvem do mundo real, uma das coisas que você precisa pensar é como lidar com interrupções de serviço temporário. Esse problema é importante exclusivamente em aplicativos de nuvem, porque você está tão dependente de conexões de rede e serviços externos. Com frequência, você pode obter pequenos problemas que normalmente são autorrecuperação, e se você não estiver preparado para tratá-las de forma inteligente, eles serão resultam em uma experiência ruim para seus clientes.

## <a name="causes-of-transient-failures"></a>Causas de falhas transitórias

No ambiente de nuvem você descobrirá que falharam e quedas de conexões de banco de dados ocorrer periodicamente. Isso é parcialmente porque você vai por meio de balanceadores de carga mais em comparação comparadas o ambiente local em que o seu servidor web e o servidor de banco de dados tem uma conexão física direta. Além disso, às vezes, quando você está dependente de um serviço multilocatário verá chamadas para o serviço ficam lentos ou tempo limite porque outra pessoa que usa o serviço está atingindo muito. Em outros casos, talvez o usuário que está atingindo o serviço com muita frequência, e o serviço limita você – nega conexões – deliberadamente para impedir a afetar negativamente outros locatários do serviço.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Use a lógica de repetição/retirada inteligente para reduzir o efeito de falhas transitórias

Em vez de gerar uma exceção e exibindo uma página de erro ou não está disponível para o seu cliente, você pode reconhecer os erros que são normalmente temporários e automaticamente de repetir a operação que resultou no erro, em espera que antes longa que seja bem-sucedida. Na maioria das vezes, a operação seja bem-sucedida na segunda tentativa, e você vai recuperar do erro sem o cliente nunca ter ficado ciente de que houve um problema.

Há várias maneiras que você pode implementar a lógica de repetição inteligentes.

- A Microsoft Patterns &amp; práticas recomendadas grupo tem um [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) que faz tudo para você se você estiver usando o ADO.NET para acesso de banco de dados SQL (não por meio do Entity Framework). Você acabou de definir uma política de novas tentativas – quantas vezes para tentar novamente uma consulta ou comando e por quanto tempo a esperar entre tentativas – e quebra automática de linha de seu SQL código em um *usando* bloco.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Também suporta TFH [Cache na função do Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) e [do barramento de serviço](https://azure.microsoft.com/services/service-bus/).
- Quando você usa o Entity Framework você normalmente não está trabalhando diretamente com conexões de SQL, portanto, você não pode usar este pacote de padrões e práticas recomendadas, mas o Entity Framework 6 cria esse tipo de lógica de repetição diretamente na estrutura. Da mesma forma que você especificar a estratégia de repetição e, em seguida, o EF usa essa estratégia sempre que ele acessa o banco de dados.

    Para usar esse recurso no aplicativo Fix It, tudo o que precisamos fazer é adicionar uma classe que deriva de *DbConfiguration* e ativar a lógica de repetição.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Para exceções de banco de dados SQL que o framework identifica como erros transitórios normalmente, o código mostrado instrui o EF para tentar novamente a operação de até 3 vezes, com um atraso de retirada exponencial entre repetições e um atraso máximo de 5 segundos. Retirada exponencial significa que após cada tentativa com falha ele aguardará por um período maior de tempo antes de tentar novamente. Se três tentativas em uma linha falharem, ele lançará uma exceção. A seção a seguir sobre os disjuntores explica por que você deseja a retirada exponencial e um número limitado de novas tentativas.

    Quando você estiver usando o serviço de armazenamento do Azure, como o aplicativo Fix It faz para Blobs, e a API do cliente de armazenamento .NET já implementa o mesmo tipo de lógica, você pode ter problemas semelhantes. Basta você especifica a política de repetição ou até mesmo que você não precisa fazer isso, se você estiver satisfeito com as configurações padrão.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disjuntores

Há várias razões por que você não deseja tentar novamente muitas vezes em um período muito longo:

- Usuários demais, persistentemente repetir solicitações com falha podem prejudicar a experiência de outros usuários. Se a milhões de pessoas estão todos fazendo repetidas repetir solicitações pode ser prender filas de expedição do IIS e impedindo que seu aplicativo atendendo às solicitações que ele pode manipular com êxito.
- Se todas as pessoas está tentando novamente devido a uma falha de serviço, há pôde tantas solicitações em fila-se de que o serviço é inundado quando ele começa a recuperar.
- Se o erro é devido à limitação e há uma janela de tempo que o serviço usa para limitação, novas tentativas contínuas foi possível mover a janela e fazer com que a limitação continuar.
- Você pode ter um usuário que está esperando uma página da web para processar. Tomada de pessoas espere muito tempo talvez mais irritante que relativamente rápido aconselhá-los para tentar novamente mais tarde.

Retirada exponencial aborda algumas desses problema a limitar a frequência de repetições para que um serviço pode obter do seu aplicativo. Mas você também precisa ter *disjuntores*: isso significa que, em um determinado Repita limite de seu aplicativo parará de repetir e executa outra ação, por exemplo, um dos seguintes:

- Fallback personalizado. Se você não pode receber um preço de estoque de Reuters, talvez você pode obtê-lo da Bloomberg; ou, se você não pode receber dados do banco de dados, talvez você pode obtê-lo do cache.
- Falha silenciosa. Se você precisa de um serviço não for tudo ou nada para seu aplicativo, basta retorne nulo quando você não pode obter os dados. Se você estiver exibindo uma tarefa Fix It e o serviço Blob não está respondendo, você pode exibir os detalhes da tarefa sem a imagem.
- Falhe rapidamente. O usuário para evitar sobrecarregar o serviço com um erro repetir solicitações que poderiam causar a interrupção do serviço para outros usuários ou estender um período de limitação. Você pode exibir uma mensagem amigável "tente novamente mais tarde".

Não há nenhuma política de repetição padronizada. Você pode repetir mais vezes e esperar mais de um processo de trabalho assíncrono em segundo plano que se estivesse em um aplicativo web síncrono em que um usuário está aguardando uma resposta. Você pode esperar mais entre as repetições para um serviço de banco de dados relacional que você faria para um serviço de cache. Aqui estão políticas para lhe dar uma ideia de como os números podem variar de repetição de algum exemplo recomendado. (Nenhum atraso antes da primeira repetição significa "Fast primeiro".

![Políticas de repetição de exemplo](transient-fault-handling/_static/image1.png)

Para orientação de política de repetição de banco de dados SQL, consulte [solucionar problemas de falhas transitórias e erros de conexão para banco de dados SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumo

Uma estratégia de repetição/retirada pode ajudar a tornar erros temporários invisível para o cliente na maioria das vezes, e a Microsoft fornece estruturas que você pode usar para minimizar o trabalho de implementar uma estratégia de se você estiver usando o ADO.NET, Entity Framework ou o Azure Serviço de armazenamento.

No [capítulo seguinte](distributed-caching.md), vamos examinar como melhorar o desempenho e confiabilidade usando o cache distribuído.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

Documentação

- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy. Semelhante a série à prova de falhas, mas entra em mais detalhes de instruções. Consulte a seção de telemetria e diagnóstico.
- [Failsafe: Diretrizes para arquiteturas resilientes na nuvem](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper, Marc Mercuri, Ulrich Homann e Andrew Townhill. Versão da página da Web da série de vídeo à prova de falhas.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte de repetição padrão, o padrão de Supervisor de agente do Agendador.
- [Tolerância a falhas no banco de dados SQL do Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Postagem de blog de Tony Petrossian.
- [Entity Framework - resiliência de Conexão / lógica de repetição](https://msdn.microsoft.com/data/dn456835). Como usar e personalizar o transient fault handling recurso do Entity Framework 6.
- [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Em quarto lugar em uma série de tutoriais de nove partes, mostra como configurar o recurso de resiliência de conexão de EF 6 para o banco de dados SQL.

Vídeos

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Consulte a discussão dos disjuntores no episódio 3 começando em 40:55.
- [Grande de construção: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre a criação de falha, transitória de falha de tratamento e instrumentar tudo.

Exemplo de código

- [Conceitos fundamentais do serviço no Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemplo de aplicativo criado pelo Microsoft Azure equipe consultiva para clientes que demonstra como usar o [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Para obter mais informações, consulte [serviço conceitos básicos de dados Access camada de nuvem — tratamento de falhas transitórias](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH é recomendado para acesso de banco de dados usando o ADO.NET diretamente (sem usar o Entity Framework).

> [!div class="step-by-step"]
> [Anterior](monitoring-and-telemetry.md)
> [Próximo](distributed-caching.md)
