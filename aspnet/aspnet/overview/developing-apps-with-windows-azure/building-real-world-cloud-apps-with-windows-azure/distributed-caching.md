---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: (Criando aplicativos de nuvem do mundo Real com o Azure) de cache distribuído | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: aspnetcontent
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: a165c789ae656025934bc5e3ed8e8caef1c21787
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821613"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Cache (construção reais na nuvem aplicativos distribuídos com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


O capítulo anterior examinou o tratamento de falhas transitórias e mencionado no cache como uma estratégia de disjuntor. Este capítulo fornece mais informações sobre o armazenamento em cache, incluindo quando usá-lo, padrões comuns para usá-la e como implementá-lo no Azure.

## <a name="what-is-distributed-caching"></a>O que é cache distribuído

Um cache fornece alta taxa de transferência, acesso de baixa latência aos dados de aplicativo frequentemente acessadas, armazenando os dados na memória. Para um aplicativo de nuvem, o tipo mais úteis de cache é cache distribuído, o que significa que os dados não são armazenados na memória do servidor web individuais, mas em outros recursos de nuvem e os dados em cache são disponibilizados para todos os servidores de web do aplicativo (ou outra nuvem VMs que ar e usado pelo aplicativo).

![Diagrama mostrando vários servidores web, acessando os mesmos servidores de cache](distributed-caching/_static/image1.png)

Quando o aplicativo é dimensionado adicionando ou removendo servidores, ou quando servidores são substituídos devido a atualizações ou falhas, os dados em cache permanecem acessíveis para cada servidor que executa o aplicativo.

Ao evitar o acesso a dados com alta latência de um repositório de dados persistentes, caching pode melhorar drasticamente capacidade de resposta do aplicativo. Por exemplo, recuperar dados do cache é muito mais rápido que recuperá-los de um banco de dados relacional.

Um benefício adicional de armazenamento em cache é reduzido os encargos de tráfego para o armazenamento de dados persistentes, que pode resultar em custos mais baixos, quando há a saída de dados para o armazenamento de dados persistentes.

## <a name="when-to-use-distributed-caching"></a>Quando usar o cache distribuído

Armazenamento em cache funciona melhor para cargas de trabalho de aplicativo que faça a leitura mais que a gravação de dados e quando o modelo de dados é compatível com a organização de chave/valor que você usa para armazenar e recuperar dados em cache. Também é mais útil quando os usuários do aplicativo compartilham muitos dados comuns; Por exemplo, cache não fornecem benefícios tantos se cada usuário normalmente recupera dados exclusivos para esse usuário. Um exemplo em que o cache pode ser muito útil é um catálogo de produtos, porque os dados não mudam frequentemente e todos os clientes estão observando os mesmos dados.

O benefício de armazenamento em cache se torna cada vez mais mensurável mais um aplicativo pode ser dimensionado, como os limites de taxa de transferência e os atrasos de latência de armazenamento de dados persistentes tornam-se mais de um limite de desempenho geral do aplicativo. No entanto, você pode implementar o cache por outros motivos que o desempenho também. Para dados não precisam ser perfeitamente atualizado quando mostrada ao usuário, o acesso ao cache pode servir como um disjuntor para quando o armazenamento de dados persistente não está respondendo ou não está disponível.

## <a name="popular-cache-population-strategies"></a>Estratégias de população de cache populares

Para poder recuperar dados do cache, você precisa para armazená-la lá primeiro. Há várias estratégias para obtenção de dados que você precisa em um cache:

- Sob demanda / Cache Aside

    O aplicativo tenta recuperar dados do cache e quando o cache não tem os dados (um "Srta"), o aplicativo armazena os dados no cache para que ele estará disponível na próxima vez. Na próxima vez que o aplicativo tenta obter os mesmos dados, ele localiza o que está procurando no cache (um "acertar"). Para evitar que buscar os dados armazenados em cache que foi alterado no banco de dados, você invalida o cache ao fazer alterações ao repositório de dados.
- Envio de dados em segundo plano

    Serviços em segundo plano dados por push o cache em um agendamento regular e o aplicativo sempre recebe do cache. Essa abordagem funciona bem com fontes de dados de alta latência que não exigem que você sempre retorna os dados mais recentes.
- Disjuntor

    O aplicativo normalmente se comunica diretamente com o armazenamento de dados persistentes, mas quando o armazenamento de dados persistentes tem problemas de disponibilidade, o aplicativo recupera dados do cache. Dados podem ter sido colocados no cache usando cache à parte ou estratégia de envio por push de dados em segundo plano. Essa é a estratégia em vez de uma estratégia de melhoria de desempenho de controle de falhas.

Para manter dados no cache atual, você pode excluir as entradas de cache relacionados ao seu aplicativo cria, atualiza, ou exclui dados. Se ele estiver muito bem para seu aplicativo, às vezes, obter dados que é um pouco desatualizados, você pode confiar em um tempo de validade configurável para definir um limite no cache de quanto tempo os dados podem ser.

Você pode configurar uma expiração absoluta (período de tempo desde que o item de cache foi criado) ou deslizante validade (período de tempo desde a última vez em que um item de cache foi acessado). Expiração absoluta é usada quando você estiver dependendo do mecanismo de expiração de cache para impedir que os dados se torne muito desatualizado. No aplicativo Fix It, podemos removerão manualmente os itens de cache obsoletos e usaremos a expiração deslizante para manter os dados mais atuais no cache. Independentemente da política de expiração que você escolher, o cache removerá automaticamente os itens mais antigos (menos usadas recentemente ou LRU) quando é atingido o limite de memória do cache.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Exemplo de código de cache-aside para corrigir o aplicativo

No código de exemplo a seguir, verificamos o cache pela primeira vez ao recuperar uma tarefa para corrigi-lo. Se a tarefa for encontrada no cache, retornamos Se não for encontrado, podemos obtê-la no banco de dados e armazená-lo no cache. As alterações que você faria para adicionar o cache para o `FindTaskByIdAsync` método são realçadas.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Quando você atualiza ou excluir uma tarefa para corrigi-lo, você precisa invalidar tarefa (remover) o armazenados em cache. Caso contrário, o futuro tenta ler que essa tarefa continuará obter os dados antigos do cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Esses são exemplos para ilustrar o código de cache simple; armazenamento em cache não foi implementado no download Fix It projeto.

## <a name="azure-caching-services"></a>Serviços de cache do Azure

O Azure oferece os seguintes serviços de cache: [Cache Redis do Azure](https://msdn.microsoft.com/library/dn690523.aspx) e [Cache gerenciado do Azure](https://msdn.microsoft.com/library/dn386094.aspx). O cache Redis do Azure é baseado no popular [software livre Cache Redis](http://redis.io/) e é a primeira opção para a maioria dos cenários de cache.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Estado da sessão ASP.NET usando um provedor de cache

Conforme mencionado na [capítulo de práticas recomendado do web desenvolvimento](web-development-best-practices.md), uma prática recomendada é evitar o uso do estado de sessão. Se seu aplicativo requer que o estado de sessão, a próxima melhor prática é evitar o provedor na memória padrão porque o que não permite expansão (várias instâncias do servidor web). O provedor de estado de sessão do SQL Server ASP.NET permite que um site que é executado em vários servidores web para usar o estado de sessão, mas ela acarreta um custo de latência alta em comparação comparado um provedor na memória. A melhor solução, se você tiver que usar o estado de sessão é usar um provedor de cache, como o [provedor de estado de sessão para Cache do Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Resumo

Você viu como o aplicativo Fix It poderá implementar o cache para melhorar a escalabilidade e tempo de resposta e para habilitar o aplicativo continue a ser ágeis na resposta para operações de leitura quando o banco de dados estiver disponível. No [capítulo seguinte](queue-centric-work-pattern.md) , mostraremos como melhorar a escalabilidade e tornar o aplicativo continue a ser ágeis na resposta para operações de gravação.

## <a name="resources"></a>Recursos

Para obter mais informações sobre o cache, consulte os seguintes recursos.

Documentação

- [O Cache do Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentação oficial do MSDN em cache no Azure.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte as diretrizes do cache e o padrão Cache-Aside.
- [Failsafe: Diretrizes para arquiteturas resilientes na nuvem](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper, Marc Mercuri, Ulrich Homann e Andrew Townhill. Consulte a seção sobre cache.
- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. White paper, Mark Simms e Michael Thomassy. Consulte a seção sobre armazenamento em cache distribuído.
- [O caminho para escalabilidade cache distribuído](https://msdn.microsoft.com/magazine/dd942840.aspx). Um artigo da MSDN Magazine (2009) mais antigo, mas uma introdução escrita com clareza para armazenamento em cache distribuído em geral; entra em mais detalhes do que as seções de cache dos white papers à prova de falhas e as práticas recomendadas.

Vídeos

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta uma exibição de nível 400 de como projetar aplicativos de nuvem. Esta série se concentra na teoria e motivos por que; Para obter mais detalhes de instruções, consulte a série Big construção por Mark Simms. Consulte a discussão de cache no episódio 3 começando em 1:24:14.
- [Grande de construção: Lições aprendidas de clientes do Azure – parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discute começando de cache distribuído em 46:00. Semelhante a série à prova de falhas, mas entra em mais detalhes de instruções. A apresentação foi atribuída a 31 de outubro de 2012, portanto, ele não aborda o serviço de cache de aplicativos Web no serviço de aplicativo do Azure que foi introduzido em 2013.

Exemplo de código

- [Conceitos fundamentais do serviço no Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo que implementa o cache distribuído. Consulte a postagem de blog que acompanha este artigo [conceitos básicos de serviço de nuvem – conceitos básicos do armazenamento em cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Anterior](transient-fault-handling.md)
> [Próximo](queue-centric-work-pattern.md)
