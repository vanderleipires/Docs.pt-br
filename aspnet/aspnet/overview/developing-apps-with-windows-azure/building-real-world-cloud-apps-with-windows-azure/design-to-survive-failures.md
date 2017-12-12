---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Design sobreviver a falhas (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: a0ee790da07c99cdb1279a6bca637a4ce8076e84
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Design sobreviver a falhas (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Uma das coisas que você deve considerar ao criar qualquer tipo de aplicativo, mas especialmente um que será executado na nuvem em que muitas pessoas usarão, é como criar o aplicativo para que ele normalmente pode lidar com falhas e continuar a fornecer um valor máximo é possível. Coisas devido tempo suficiente, vai errar em qualquer ambiente ou em qualquer sistema de software. Como o seu aplicativo lida com essas situações determina como aborrecida terão seus clientes e o tempo gasto analisando e corrigir problemas.

## <a name="types-of-failures"></a>Tipos de falhas

Há duas categorias básicas de falhas que você vai querer controlar de maneira diferente:

- Transitório, reparo automático de falhas, como problemas de conectividade de rede intermitente.
- Falhas de constante que requerem intervenção.

Para falhas transitórias, você pode implementar uma política de repetição para garantir que a maior parte do tempo que o aplicativo recupera de forma rápida e automática. Seus clientes poderá notar um pouco mais tempo de resposta, mas, caso contrário, eles não serão afetados. Vamos mostrar algumas maneiras de tratar esses erros no [capítulo tratamento de falhas transitórias](transient-fault-handling.md).

Para manter a falhas, você pode implementar o monitoramento e registro em log a funcionalidade que notifica você imediatamente quando surgem problemas e que facilita a análise da causa raiz. Mostraremos algumas maneiras de ajudá-lo sobre esses tipos de erros de [capítulo de monitoramento e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Escopo da falha

Você também precisa pensar sobre o escopo da falha – se um único computador é afetado, um serviço inteiro como uma região inteira, armazenamento ou banco de dados SQL.

![Escopo da falha](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Falhas de máquina

No Azure, um servidor com falha é substituído automaticamente por um novo, e um aplicativo de nuvem bem projetada recupera desse tipo de falha rapidamente e automaticamente. Anteriormente, enfatizado os benefícios de escalabilidade de uma camada da web sem monitoração de estado e a facilidade de recuperação de um servidor com falha é outro benefício de sem estado. Facilidade de recuperação também é um dos benefícios dos recursos do plataforma como serviço (PaaS), como o banco de dados SQL e aplicativos de Web do serviço de aplicativo do Azure. Falhas de hardware são raras, mas quando ocorrem que esses serviços tratá-los automaticamente. Você ainda não precisa escrever código para lidar com falhas de computador quando você estiver usando um desses serviços.

### <a name="service-failures"></a>Falhas de serviço

Aplicativos de nuvem normalmente usam vários serviços. Por exemplo, o aplicativo corrigir usa o serviço de banco de dados SQL, o serviço de armazenamento e o aplicativo web é implantado para o serviço de aplicativo do Azure. O que seu aplicativo será fazer se um dos serviços que depende de você falhar? Para alguns serviços falhas amigável para um ", tente novamente mais tarde" mensagem pode ser a melhor a fazer. Mas, em muitos cenários você pode fazer melhor. Por exemplo, quando o armazenamento de dados de back-end estiver inativo, você pode aceitar a entrada do usuário, exibir "sua solicitação foi recebida" e armazene a entrada em algum lugar else temporariamente; em seguida, quando o serviço é necessário está funcionando novamente, você pode recuperar a entrada e processá-la.

O [padrão centrado em fila de trabalho](queue-centric-work-pattern.md) capítulo mostra uma maneira de lidar com esse cenário. O aplicativo corrigir armazena tarefas no banco de dados SQL, mas não é necessário que parem de funcionar quando o banco de dados SQL está inativo. Esse capítulo veremos como armazenar a entrada do usuário para uma tarefa em uma fila e usar um processo de trabalho para ler a fila e a tarefa de atualização. Se SQL estiver inativo, a capacidade de criar tarefas corrigir é afetada; o processo de trabalho pode esperar e processar novas tarefas quando o banco de dados SQL está disponível.

### <a name="region-failures"></a>Falhas de região

Regiões inteiras podem falhar. Um desastre natural pode destruir um data center, ele pode obter nivelado por um meteor, a linha de tronco no data center foi retirada por um farmer burying um cow com um escavadeira, etc. Se seu aplicativo estiver hospedado no datacenter afetada tenha o que fazer? É possível configurar seu aplicativo no Azure sejam executados em várias regiões simultaneamente para que se houver um desastre em um, continuar em execução em outra região. Essas falhas são extremamente raras ocorrências e a maioria dos aplicativos não passar através de meios necessários para garantir a continuidade do serviço por meio de falhas desse tipo. Consulte a seção de recursos no final deste capítulo para obter informações sobre como manter seu aplicativo disponível mesmo por meio de uma falha de região.

Um objetivo do Azure é a manipulação de todos os tipos de falhas muito mais fácil, e você verá alguns exemplos de como estamos fazendo isso nos capítulos a seguir.

## <a name="slas"></a>SLAs

As pessoas geralmente informações sobre contratos de nível de serviço (SLAs) no ambiente de nuvem. Basicamente, essas são promessas daquele empresas sobre seu serviço está como confiável. 99,9% SLA significa que você deve esperar que o serviço estar funcionando corretamente 99,9% do tempo. Que é um valor bastante comum para um SLA e ele se parece com um número muito alto, mas você talvez não percebam quanto tempo de inatividade. % 1, na verdade, valores para. Aqui está uma tabela que mostra quanto tempo de inatividade várias porcentagens de SLA valor para em um ano, mês e semana.

![Tabela de SLA](design-to-survive-failures/_static/image2.png)

Portanto um SLA significa que seu serviço de 99,9% podem estar inativos horas 8.76 43,2 minutos por mês ou um ano. Isso é mais tempo de inatividade que a maioria das pessoas imagina. Assim, como um desenvolvedor deseja Lembre-se de que uma determinada quantidade de tempo de inatividade, é possível e tratá-la de uma maneira normal. Em algum momento alguém vai ser usando o seu aplicativo e um serviço vai estar desativado e você quer minimizar o impacto negativo do que no cliente.

Uma coisa que você deve saber sobre um SLA é o período de tempo se refere a: o relógio ser redefinido toda semana, mensalmente ou anualmente? No Azure redefinimos o relógio de cada mês, o que é melhor para você do que um SLA anual, já que um SLA anual pode ocultar ruim deslocando-los com uma série de meses BOM.

Obviamente, sempre pretender fazer melhor do que o SLA; Normalmente, você estará inativo muito menos do que. A promessa é que se estamos nunca por mais do que o máximo de tempo de inatividade, você pode pedir dinheiro de volta. A quantidade de dinheiro que você voltar provavelmente não compense por completo o impacto de negócios da taxa de excesso de tempo de inatividade, mas esse aspecto do SLA atua como uma política de imposição e permite que você saiba que estejamos sério.

### <a name="composite-slas"></a>SLAs compostos

Uma coisa importante a considerar quando você está olhando SLAs é o impacto de usar vários serviços em um aplicativo, com cada serviço tem um SLA separado. Por exemplo, o aplicativo corrigir usa aplicativos de Web do serviço de aplicativo do Azure, o armazenamento do Azure e o banco de dados SQL. Aqui estão os números de SLA a partir da data em que este livro eletrônico está sendo gravado em dezembro de 2013:

![Banco de dados SQL do SLA Site da Web, armazenamento,](design-to-survive-failures/_static/image3.png)

O que é o máximo de tempo de inatividade esperado para o aplicativo com base em SLAs desses serviços? Você pode achar que seu tempo de inatividade seria igual a porcentagem de SLA pior ou 99,9% nesse caso. Que poderá ocorrer se todos os três serviços sempre falhou ao mesmo tempo, mas que não é necessariamente o que realmente acontece. Cada serviço pode falhar independentemente em momentos diferentes, você precisa calcular o SLA composto multiplicando os números de SLA individuais.

![SLA composto](design-to-survive-failures/_static/image4.png)

Para que seu aplicativo pode estar inativo não apenas 43,2 minutos, 3 vezes esse valor, mas um mês 108 minutos por mês – e permanecer dentro dos limites de SLA do Azure.

Esse problema não é exclusivo no Azure. Na verdade, fornecemos melhor nuvem SLAs de qualquer serviço de nuvem disponível e você terá problemas semelhantes para lidar com se você usar os serviços de nuvem de qualquer fornecedor. O que isso destaca é a importância de pensar sobre como você pode projetar seu aplicativo para tratar as falhas de serviço inevitável normalmente, porque podem ocorrer com frequência suficiente afetar seus clientes ou usuários.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Em comparação comparados a experiência de tempo de inatividade de empresa de SLAs de nuvem

As pessoas às vezes, digamos, "No meu aplicativo empresarial que nunca ter esses problemas." Se você pedir quanto tempo de inatividade por mês, na verdade, têm, normalmente dizem, "Bem, isso acontece ocasionalmente." E se você fizer com que frequência, admitir que "Às vezes precisamos fazer backup ou instalar um novo servidor ou atualização de software." É claro que conta como tempo de inatividade. A maioria dos aplicativos da empresa, a menos que elas são especialmente críticos são realmente inativo por mais do que a quantidade de tempo permitido pelo nosso serviço SLAs. Mas, quando ele for seu servidor e sua infraestrutura e é responsável por ele e no controle do mesmo, você tende a se sentir menos angst para baixo vezes. Em um ambiente de nuvem é dependente de outra pessoa e você não souber o que está acontecendo, portanto você pode tendem a ter mais preocupado com ele.

Quando uma empresa alcança um percentual de tempo maior do que o obtido de uma nuvem SLA, eles fazem gastar muito mais dinheiro em hardware. Um serviço de nuvem pode fazer isso, mas teria a cobrar muito mais de seus serviços. Em vez disso, você pode tirar proveito de um serviço de baixo custo e projetar seu software para que as falhas inevitáveis causam a interrupção mínima aos seus clientes. Seu trabalho como um designer de aplicativo de nuvem não é muito mais para evitar falhas para evitar catástrofes e fazer isso, concentrando-se sobre software, não em hardware. Enquanto os aplicativos corporativos se esforçam para maximizar o tempo médio entre falhas, aplicativos de nuvem se esforçam para minimizar o tempo médio de recuperação.

### <a name="not-all-cloud-services-have-slas"></a>Nem todos os serviços de nuvem tem SLAs

Lembre-se também que nem todos os serviços de nuvem tem até mesmo um SLA. Se seu aplicativo depende de um serviço com nenhuma garantia de tempo de atividade, você pode estar desativado muito mais do que você pode imaginar. Por exemplo, se você habilitar o logon para seu site usando um provedor social, como o Facebook ou Twitter, verifique com o provedor de serviços para descobrir se há um SLA e talvez você ache lá não. Mas, se o serviço de autenticação é desativado ou não puder suportar o volume de solicitações desejado, os clientes estão bloqueados de seu aplicativo. Você pode estar desativado por dias ou mais. Os criadores de um novo aplicativo esperado centenas de milhões de downloads e obteve uma dependência na autenticação do Facebook – mas não se comunicar com o Facebook antes de ir ao vivo e descobertos muito tarde que não houve nenhum SLA para o serviço.

### <a name="not-all-downtime-counts-toward-slas"></a>Nem todas as contagens de tempo de inatividade para SLAs

Alguns serviços de nuvem deliberadamente podem negar serviço se seu aplicativo usa excesso-los. Isso é chamado de *limitação*. Se um serviço tem um SLA, ele deve indicar as condições em que você pode ser limitada e seu design de aplicativo deve evitar essas condições e reagir de forma adequada para a limitação se ele ocorre. Por exemplo, se as solicitações para um serviço começam a falhar ao exceder um determinado número por segundo, você deseja certificar-se de novas tentativas automáticas não acontecem rápida que causem a limitação continuar. Teremos mais a dizer sobre limitação no [capítulo tratamento de falhas transitórias](transient-fault-handling.md).

## <a name="summary"></a>Resumo

Este capítulo tentou ajudam você a compreender por que um aplicativo de nuvem do mundo real deve ser projetado para sobreviver a falhas normalmente. Começando com o [próximo capítulo](monitoring-and-telemetry.md), os padrões restantes nesta série entram em mais detalhes sobre algumas estratégias que você pode usar para fazer isso:

- Ter bom [monitoramento e telemetria](monitoring-and-telemetry.md), de modo que você descobrir rapidamente sobre falhas que requerem intervenção e você tem informações suficientes para resolvê-los.
- [Tratar falhas transitórias](transient-fault-handling.md) implementando lógica de repetição inteligentes, para que seu aplicativo recupera automaticamente quando possível e reverterá para [disjuntor](transient-fault-handling.md#circuitbreakers) lógica quando ele não é possível.
- Use [cache distribuído](distributed-caching.md) para minimizar problemas de conexão, latência e taxa de transferência com o acesso de banco de dados.
- Implementar flexível acoplamento o [centrado em fila de trabalho padrão](queue-centric-work-pattern.md), de modo que o front-end do aplicativo pode continuar a trabalhar quando o back-end está inativo.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte capítulos este livro eletrônico e os recursos a seguir.

Documentação:

- [À prova de falhas: Orientação para arquiteturas resilientes na nuvem](https://msdn.microsoft.com/en-us/library/windowsazure/jj853352.aspx). White paper Marc Mercuri, Ulrich Homann e Andrew Townhill. Versão de página da Web da série de vídeo à prova de falhas.
- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy.
- [Orientação técnica de continuidade de negócios do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh873027.aspx). White paper, Patrick Wickline e Jason Roth.
- [Recuperação de desastres e alta disponibilidade para aplicativos do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/dn251004.aspx). White paper, Jason Roth, Hanu Kommalapati e Michael McKeown.
- [Padrões e práticas - diretrizes do Azure Microsoft](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Consulte as diretrizes de implantação de vários Data Center, o padrão de disjuntor.
- [Suporte do Azure - contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/).
- [Continuidade dos negócios no banco de dados SQL do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx). Documentação sobre banco de dados SQL alta disponibilidade e desastres recursos de recuperação.
- [Alta disponibilidade e recuperação de desastres do SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/en-us/library/windowsazure/jj870962.aspx).

Vídeos:

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Episódios 1 e 8 entram em profundidade os motivos para criar aplicativos de nuvem para sobreviver a falhas. Consulte também a discussão de acompanhamento de limitação no episódio 2 começando em 49:57, a discussão de pontos de falha e modos de falha no episódio 2 começando em 56:05 e a discussão de disjuntores no episódio 3 começando em 40:55.
- [Criando Big: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre a criação da falha e instrumentação tudo. Semelhante ao entrar em mais detalhes instruções mas série à prova de falhas.

>[!div class="step-by-step"]
[Anterior](unstructured-blob-storage.md)
[Próximo](monitoring-and-telemetry.md)
