---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Design para resistir a falhas (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 1098892c621b8add624b18788076be2fac1cc158
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823493"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Design para resistir a falhas (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Uma das coisas que você precisa pensar quando você compilar qualquer tipo de aplicativo, mas especialmente um que será executado na nuvem em que muitas pessoas usarão, é como projetar o aplicativo para que ele normalmente pode lidar com falhas e continuar a fornecer o valor máximo é possível. Dada a tempo suficiente, as coisas vão dar errado em qualquer ambiente ou em qualquer sistema de software. Como o seu aplicativo lida com essas situações determina como aborrecida obterá seus clientes e o tempo gasto analisando e correção de problemas.

## <a name="types-of-failures"></a>Tipos de falhas

Há duas categorias básicas de falhas que você vai querer manipular de forma diferente:

- Transitórias, falhas, como problemas de conectividade de rede intermitente de autorrecuperação.
- Falhas de multifacetada que requerem intervenção.

Para falhas transitórias, você pode implementar uma política de repetição para garantir que a maioria das vezes o aplicativo será recuperado, de forma rápida e automática. Seus clientes podem observar o tempo de resposta um pouco maior, mas caso contrário, eles não serão afetados. Vamos mostrar algumas maneiras de lidar com esses erros na [capítulo de tratamento de falhas transitórias](transient-fault-handling.md).

Para duradouro falhas, você pode implementar o monitoramento e registro em log a funcionalidade que notifica você imediatamente quando surgirem problemas e facilita a análise da causa raiz. Vamos mostrar algumas maneiras de ajudá-lo sobre esses tipos de erros na [capítulo de monitoramento e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Escopo da falha

Você também precisa pensar sobre o escopo de falha – se um único computador é afetado, um serviço inteiro, como o banco de dados SQL ou armazenamento ou uma região inteira.

![Escopo da falha](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Falhas de máquina

No Azure, um servidor com falha automaticamente é substituído por um novo e um aplicativo de nuvem bem projetado recupera desse tipo de falha rápida e automaticamente. Anteriormente é enfatizado os benefícios de escalabilidade de uma camada da web sem monitoração de estado e a facilidade de recuperação a partir de um servidor com falha é outro benefício de sem estado. Facilidade de recuperação também é um dos benefícios dos recursos do plataforma como serviço (PaaS), como banco de dados SQL e aplicativos de Web do serviço de aplicativo do Azure. Falhas de hardware são raras, mas quando ocorrem que esses serviços tratarão-las automaticamente. até mesmo, você não precisa escrever código para lidar com falhas de computador quando você estiver usando um desses serviços.

### <a name="service-failures"></a>Falhas de serviço

Aplicativos de nuvem normalmente usam vários serviços. Por exemplo, o aplicativo Fix It usa o serviço de banco de dados SQL, o serviço de armazenamento e o aplicativo web é implantado no serviço de aplicativo do Azure. O que seu aplicativo fará se um dos serviços que depende de você falhar? Para alguns serviço falhas um amigável "Desculpe, tente novamente mais tarde" mensagem pode ser a melhor que você pode fazer. Mas, em muitos cenários você pode fazer melhor. Por exemplo, quando o armazenamento de dados de back-end estiver inativo, você pode aceitar a entrada do usuário, exibir "sua solicitação foi recebida" e armazenar a entrada em algum lugar else temporariamente; em seguida, quando o serviço que você precisa está funcionando novamente, você pode recuperar a entrada e processá-lo.

O [padrão de trabalho centrado em fila](queue-centric-work-pattern.md) capítulo mostra uma maneira de lidar com esse cenário. O aplicativo Fix It armazena as tarefas no banco de dados SQL, mas não é necessário que parem de funcionar quando o banco de dados SQL está inativo. Esse capítulo, veremos como armazenar a entrada do usuário para uma tarefa em uma fila e usar um processo de trabalho para ler a fila e atualizar a tarefa. Se SQL estiver inativo, a capacidade de criar tarefas Fix It é afetada; o processo de trabalho pode aguardar e processar novas tarefas quando o banco de dados SQL está disponível.

### <a name="region-failures"></a>Falhas de região

Regiões inteiras podem falhar. Um desastre natural pode destruir um data center, ele pode obter nivelado por um meteor, a linha de tronco para o datacenter poderão ser cortada por um farmer burying uma vaca com uma escavadeira, etc. Se seu aplicativo estiver hospedado no datacenter afetada tenha o que fazer? É possível configurar seu aplicativo no Azure para executar em várias regiões ao mesmo tempo para que se houver um desastre em um, continuar em execução em outra região. Essas falhas são extremamente raras, e a maioria dos aplicativos não passar pelos obstáculos necessários para garantir a continuidade do serviço por meio de falhas desse tipo. Consulte a seção de recursos no final deste capítulo para obter informações sobre como manter seu aplicativo disponível mesmo por meio de uma falha de região.

Um objetivo do Azure é fazer o tratamento todos esses tipos de falhas muito mais fácil, e você verá alguns exemplos de como estamos fazendo isso nos capítulos a seguir.

## <a name="slas"></a>SLAs

As pessoas geralmente ouça sobre os contratos de nível de serviço (SLAs) no ambiente de nuvem. Basicamente, essas são promessas que as empresas fazer sobre é confiável de seus serviços. 99,9% SLA significa que você deve esperar que o serviço esteja funcionando corretamente 99,9% do tempo. Que é um valor bem típico para um SLA e ele se parece um número muito alto, mas talvez você não saiba quanto tempo de inatividade. % 1, na verdade, equivale a. Aqui está uma tabela que mostra quanto tempo de inatividade vários percentuais de SLA amount ao longo de um ano, mês e uma semana.

![Tabela de SLA](design-to-survive-failures/_static/image2.png)

Portanto, um SLA significa que seu serviço de 99,9% podem estar inoperantes 8,76 horas, um ano ou 43,2 minutos por mês. Isso é mais tempo de inatividade do que a maioria das pessoas percebem. Assim, como um desenvolvedor deseja estar ciente de que uma determinada quantidade de tempo de inatividade, é possível e tratá-la de maneira normal. Em algum momento é alguém quiser usar seu aplicativo e um serviço vai ficar inativo, e você deseja minimizar o impacto negativo do que no cliente.

Uma coisa que você deve saber sobre um SLA é qual período ele se refere a: o relógio obter redefine todas as semanas, todos os meses ou todos os anos? No Azure redefinir o relógio de cada mês, o que é melhor para você que um SLA anual, uma vez que um SLA anual pode ocultar ruim deslocando-los com uma série de meses boa.

É claro sempre buscamos fazer melhor do que o SLA; Geralmente, você estará inativo muito menos do que isso. A promessa é que, se estivermos nunca inativo por mais tempo do que o máximo tempo de inatividade é possível obter dinheiro de volta. A quantidade de dinheiro que você voltar provavelmente não compense por completo o impacto nos negócios do excesso de tempo de inatividade, mas esse aspecto do SLA atua como uma política de imposição e permite que você saiba o que podemos levá-la muito a sério.

### <a name="composite-slas"></a>SLAs compostos

Uma coisa importante para pensar quando você estiver examinando os SLAs é o impacto do uso de vários serviços em um aplicativo, com cada serviço com um SLA separado. Por exemplo, o aplicativo Fix It usa aplicativos de Web do serviço de aplicativo do Azure, o armazenamento do Azure e o banco de dados SQL. Aqui estão seus números de SLA a partir da data em que este livro eletrônico está sendo gravado em dezembro de 2013:

![O banco de dados SQL de Site da Web, armazenamento, SLA](design-to-survive-failures/_static/image3.png)

O que é o máximo de tempo de inatividade esperado para o aplicativo com base nesses SLAs de serviço? Você pode pensar que seu tempo de inatividade seria igual a pior percentual de SLA ou 99,9% nesse caso. Isso seria verdadeiro se todos os três serviços sempre Falha ao mesmo tempo, mas que não é necessariamente o que realmente acontece. Cada serviço pode falhar independentemente em momentos diferentes, portanto, você precisa calcular o SLA composto multiplicando os números de SLA individuais.

![SLA composto](design-to-survive-failures/_static/image4.png)

Então, seu aplicativo pode estar inativo não apenas 43,2 minutos por mês, mas 3 vezes essa quantidade, 108 minutos por mês – e ainda ser dentro dos limites de SLA do Azure.

Esse problema não é exclusivo para o Azure. Na verdade, oferecemos a melhor nuvem SLAs de qualquer serviço de nuvem disponíveis e você terá problemas semelhantes para lidar com se você usar os serviços de nuvem de qualquer fornecedor. O que isso realça é a importância de pensar sobre como você pode projetar seu aplicativo para tratar as falhas de serviço inevitável normalmente, porque eles podem ocorrer com frequência suficiente para afetar seus clientes ou usuários.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>SLAs de nuvem em comparação comparados a experiência de tempo de inatividade do enterprise

As pessoas, às vezes, por exemplo, "Em meu aplicativo da empresa nunca tenho esses problemas." Se você perguntar quanto tempo de inatividade por mês, na verdade, tenham, normalmente dizem, "Bem, isso acontece ocasionalmente." E se a frequência com que você pergunte, admitir que "Às vezes é necessário fazer backup ou instalar um novo servidor ou atualização de software." É claro que conta como tempo de inatividade. A maioria dos aplicativos empresariais, a menos que eles são especialmente críticos são realmente inativo por mais do que a quantidade de tempo permitido por nossos contratos de serviço. Mas quando ele for seu servidor e sua infraestrutura e você é responsável por ele e controle sobre ele, você tende a se sentir menos comoção tempos de inatividade. Em um ambiente de nuvem, você é dependente de outra pessoa, e você não souber o que está acontecendo, portanto, você talvez tendem a ficar mais preocupado com ele.

Quando uma empresa alcança um percentual de tempo maior que o obtido de uma nuvem SLA, fazem isso por muito mais dinheiro de gastos em hardware. Um serviço de nuvem pode fazer isso, mas seria necessário cobrança muito mais para seus serviços. Em vez disso, você pode tirar proveito de um serviço econômico e projetar seu software para que as falhas inevitáveis causam interrupção mínima para seus clientes. Seu trabalho como um designer de aplicativos de nuvem não é tão grande para evitar falhas para evitar a catástrofe, e você faz isso, concentrando-se sobre software, não em hardware. Enquanto os aplicativos empresariais se esforça maximizar o tempo médio entre falhas, aplicativos de nuvem se esforçar para minimizar o tempo médio para recuperar.

### <a name="not-all-cloud-services-have-slas"></a>Nem todos os serviços de nuvem têm SLAs

Lembre-se também que nem todo serviço de nuvem ainda tem um SLA. Se seu aplicativo depende de um serviço sem nenhuma garantia de tempo de atividade, você pode estar desativado muito mais tempo do que você imagina. Por exemplo, se você habilitar o log em seu site usando um provedor social como Facebook ou Twitter, verifique com o provedor de serviço para descobrir se há um SLA e talvez por aí não é um. Mas, se o serviço de autenticação fica inativo ou não é capaz de suportar o volume de solicitações que você jogue para ele, os clientes estão bloqueados de seu aplicativo. Você pode estar inativo por dias ou mais. Os criadores de um novo aplicativo esperado centenas de milhões de downloads e obteve uma dependência na autenticação do Facebook –, mas não conversavam Facebook antes de ir ao vivo e descobertos tarde demais que não houve nenhum SLA para o serviço.

### <a name="not-all-downtime-counts-toward-slas"></a>Nem todas as contagens de tempo de inatividade em direção a SLAs

Alguns serviços de nuvem deliberadamente podem negar serviço se seu aplicativo usa-os em excesso. Isso é chamado *limitação*. Se um serviço tem um SLA, ele deve indicar as condições sob as quais você pode estar restrito e design de seu aplicativo deve evitar essas condições e reagir adequadamente à limitação se ele ocorre. Por exemplo, se as solicitações para um serviço começarem a falhar quando você excede um determinado número por segundo, você deseja certificar-se de novas tentativas automáticas não ocorram tão rápido que eles fazem com que a limitação continuar. Teremos mais a dizer sobre a limitação nos [capítulo de tratamento de falhas transitórias](transient-fault-handling.md).

## <a name="summary"></a>Resumo

Este capítulo tem a intenção de ajudar você a compreender por que um aplicativo de nuvem do mundo real precisa ser criado para resistir a falhas normalmente. Começando com o [capítulo seguinte](monitoring-and-telemetry.md), os padrões restantes nesta série de entram em mais detalhes sobre algumas estratégias que você pode usar para fazer isso:

- Tem bom [monitoramento e telemetria](monitoring-and-telemetry.md), para que você saiba rapidamente sobre falhas que exigem intervenção, e você tem informações suficientes para resolvê-los.
- [Tratar falhas transitórias](transient-fault-handling.md) Implementando a lógica de repetição inteligentes, para que seu aplicativo será recuperado automaticamente quando possível e reverterá para [disjuntor](transient-fault-handling.md#circuitbreakers) lógica quando não for possível.
- Use [caching distribuído](distributed-caching.md) para minimizar problemas de conexão, latência e taxa de transferência com o acesso de banco de dados.
- Implementar flexível de acoplamento por meio de [padrão centrado em fila](queue-centric-work-pattern.md), de modo que seu aplicativo front-end pode continuar a trabalhar quando o back-end está inativo.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os últimos capítulos este livro eletrônico e os recursos a seguir.

Documentação:

- [Failsafe: Diretrizes para arquiteturas resilientes na nuvem](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper, Marc Mercuri, Ulrich Homann e Andrew Townhill. Versão da página da Web da série de vídeo à prova de falhas.
- [Práticas recomendadas para o Design de serviços em grande escala em serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy.
- [Orientação técnica de continuidade de negócios do Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). White paper, Patrick Wickline e Jason Roth.
- [Recuperação de desastres e alta disponibilidade para aplicativos do Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). White paper, Michael McKeown, Hanu Kommalapati e Jason Roth.
- [Microsoft Patterns and Practices - diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte as diretrizes de implantação de vários Data Center, o padrão de disjuntor.
- [Suporte do Azure – contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/).
- [Continuidade dos negócios no banco de dados SQL do Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentação sobre banco de dados SQL alta disponibilidade e desastres recursos de recuperação.
- [Alta disponibilidade e recuperação de desastres para SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vídeos:

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Episódios 1 e 8 entram em detalhes os motivos para a criação de aplicativos de nuvem para resistir a falhas. Consulte também a discussão de acompanhamento de limitação no episódio 2 começando em 49:57, a discussão sobre pontos de falha e modos de falha no episódio 2 começando em 56:05 e a discussão dos disjuntores no episódio 3 começando em 40:55.
- [Grande de construção: Lições aprendidas de clientes do Azure - parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre design para a falha e instrumentar tudo. Semelhante a série à prova de falhas, mas entra em mais detalhes de instruções.

> [!div class="step-by-step"]
> [Anterior](unstructured-blob-storage.md)
> [Próximo](monitoring-and-telemetry.md)
