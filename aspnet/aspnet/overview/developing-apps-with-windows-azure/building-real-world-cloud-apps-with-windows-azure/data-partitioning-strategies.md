---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: "(Criando aplicativos de nuvem do mundo Real com o Azure) de estratégias de particionamento de dados | Microsoft Docs"
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 8eddb7af2d9032153b30ab54d5e882f0b46cd4ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>(Criando aplicativos de nuvem do mundo Real com o Azure) de estratégias de particionamento de dados
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre a série, consulte [primeiro capítulo](introduction.md).


Anteriormente, vimos como é fácil dimensionar a camada da web de um aplicativo de nuvem, adicionando e removendo servidores web. Mas, se eles todos atingindo o mesmo repositório de dados, move de afunilamento do aplicativo de front-end para o back-end, e a camada de dados é mais difíceis de dimensionar. Neste capítulo, vamos examinar como você pode fazer sua camada de dados escalonáveis particionando dados em vários bancos de dados relacionais ou pela combinação de armazenamento de banco de dados relacional com outras opções de armazenamento de dados.

Configurando um esquema de particionamento é melhor concluído com antecedência pela mesma razão mencionado anteriormente: é muito difícil alterar sua estratégia de armazenamento de dados depois de um aplicativo em produção. Se você pensar em disco rígido com antecedência abordagens diferentes, você pode evitar um "tempo de Twitter" quando o aplicativo falhar ou fica inativo por muito tempo enquanto você reorganizar dados e o código de acesso a dados do aplicativo.

## <a name="the-three-vs-of-data-storage"></a>Os três Vs de armazenamento de dados

Para determinar se você precisa de uma estratégia de particionamento e o que deve ser, considere as perguntas sobre seus dados:

- Volume – a quantidade de dados será você finalmente armazenar? Alguns gigabytes? Algumas centenas de gigabytes? Terabytes? Petabytes?
- Velocidade – o que é a taxa de crescem de seus dados? É um aplicativo interno que não está gerando muitos dados? Um aplicativo externo que os clientes serão carregamento de imagens e vídeos em?
- Variedade – tipo de dados serão armazenados? Relacionais, imagens, pares chave-valor, gráficos sociais?

Se você achar que você vai tem um grande volume, velocidade ou vários, você precisa considerar cuidadosamente o tipo de esquema de particionamento melhor permitirá que seu aplicativo para dimensionar com eficiência e eficácia cresce e para garantir que você não encontrar afunilamentos.

Basicamente, há três métodos de particionamento:

- Particionamento vertical
- particionamento horizontal
- Particionamento híbrido

## <a name="vertical-partitioning"></a>Particionamento vertical

Portioning vertical é como dividir uma tabela por colunas: um conjunto de colunas entra em um armazenamento de dados e outro conjunto de colunas entra em um repositório de dados diferentes.

Por exemplo, suponha que o meu aplicativo armazena dados sobre pessoas, incluindo as imagens:

![Tabela de dados](data-partitioning-strategies/_static/image1.png)

Quando você representa esses dados como uma tabela e a variedade de dados, você pode ver que as três colunas à esquerda tem dados de cadeia de caracteres que podem ser armazenados com eficiência, um banco de dados relacional, enquanto as duas colunas à direita são essencialmente as matrizes de bytes que c ome de arquivos de imagem. É possível para dados de arquivo de imagem de armazenamento em um banco de dados relacional, e muitas pessoas fazer isso, porque eles não desejarem salvar os dados para o sistema de arquivos. Talvez não tenham um sistema de arquivo capaz de armazenar necessários volumes de dados ou eles talvez não queira gerenciar um backup separado e restauração do sistema. Essa abordagem funciona bem para bancos de dados local e para pequenas quantidades de dados em bancos de dados de nuvem. No ambiente local, pode ser mais fácil deixar o DBA cuidar de tudo.

Mas em um banco de dados de nuvem, armazenamento é relativamente caro, e um alto volume de imagens pode fazer o tamanho do banco de dados ultrapassar os limites em que ele pode operar com eficiência. Você pode resolver esses problemas ao particionar os dados verticalmente, o que significa que você escolher o repositório de dados mais apropriado para cada coluna na tabela de dados. O que pode funcionar melhor para este exemplo é colocar os dados de cadeia de caracteres em um banco de dados relacional e as imagens no armazenamento de Blob.

![Tabela de dados particionada verticalmente](data-partitioning-strategies/_static/image2.png)

Armazenar imagens no armazenamento de Blob em vez de um banco de dados é mais prático na nuvem do que em um ambiente local porque você não precisa se preocupar sobre a configuração de servidores de arquivos ou o gerenciamento de backup e restauração dos dados armazenados fora do banco de dados relacional: todos os que é manipulada para você automaticamente pelo serviço de armazenamento de Blob.

Essa é a abordagem de particionamento é implementado no aplicativo corrigir e vamos examinar o código para que o [capítulo de armazenamento de Blob](unstructured-blob-storage.md). Sem esse esquema de particionamento e supondo que um tamanho de imagem média de 3 megabytes, o aplicativo corrigir só poderá armazenar aproximadamente 40.000 tarefas antes de atingir o tamanho máximo de 150 GB. Depois de remover as imagens, o banco de dados pode armazenar 10 vezes mais tarefas. Você pode ir mais tempo antes de você precisa pensar sobre como implementar um esquema de particionamento horizontal. E como o aplicativo pode ser dimensionado, suas despesas crescem mais lenta porque o de suas necessidades de armazenamento em massa vai no armazenamento de Blob muito baixo custo.

## <a name="horizontal-partitioning-sharding"></a>(Fragmentação) de particionamento horizontal

Portioning horizontal é como dividir uma tabela por linhas: um conjunto de linhas entra em um armazenamento de dados e outro conjunto de linhas entra em um repositório de dados diferentes.

Dado o mesmo conjunto de dados, outra opção seria armazenar diferentes intervalos de nomes de clientes em diferentes bancos de dados.

![Tabela de dados particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Você deseja ter muito cuidado ao seu esquema de fragmentação para certificar-se de que dados forem distribuídos uniformemente para evitar pontos de acesso. Esse exemplo simples usando a primeira letra do sobrenome não atende a esse requisito, porque muitas pessoas têm sobrenomes que começam com determinados letras comuns. Você alcance antes do que o esperado porque alguns bancos de dados tem muito grande enquanto a maioria permaneceria pequena limitações de tamanho da tabela.

Um lado de particionamento horizontal é que ele pode ser difícil fazer consultas em todos os dados. Neste exemplo, uma consulta precisa extrair de até 26 bancos de dados diferentes para obter todos os dados armazenados pelo aplicativo.

## <a name="hybrid-partitioning"></a>Particionamento híbrido

Você pode combinar o particionamento vertical e horizontal. Por exemplo, nos dados de exemplo, você pode armazenar as imagens no armazenamento de Blob e particionar horizontalmente os dados de cadeia de caracteres.

![Híbrido de tabela de dados particionado](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Particionamento de um aplicativo de produção

Conceitualmente, é fácil ver como um esquema de particionamento funcionaria, mas qualquer esquema de particionamento aumentam a complexidade de código e apresenta muitos complicações novo que você precisa lidar com. Se você estiver movendo imagens no armazenamento de blob, o que fazer quando o serviço de armazenamento? Como lidar com segurança de blob O que acontece se o armazenamento de blob e banco de dados fora de sincronização? Se você estiver fragmentação, como você administrará consultando através de todos os bancos de dados?

As complicações são gerenciáveis, contanto que você está planejando-los antes de ir para a produção. Muitas pessoas que não fez isso gostariam de ter mais tarde. Em média nossa equipe de equipe consultoria do cliente (CAT) obtém panicked chamadas telefônicas sobre uma vez por mês de clientes cujos aplicativos estão demorando de uma maneira muito grande e não preencham esse planejamento. E eles dizem algo como: "Ajuda! Posso colocar tudo em um único repositório de dados e 45 dias, vou a ficar sem espaço nele!" E se você tiver muita lógica de negócios internos como você acessa seu armazenamento de dados e você tiver clientes que estão usando o seu aplicativo, não há nenhuma bom momento para ficar inativo por um dia durante a migração. Podemos acabar passando por herculean esforços para ajudar a partição de cliente seus dados em tempo real sem nenhum tempo de inatividade. É muito empolgante e muito assustadores e não é algo que deseja estar envolvido no se for possível evitar! Pensar sobre isso com antecedência e integrá-lo em seu aplicativo tornará sua vida muito mais fácil se o aplicativo cresce mais tarde.

## <a name="summary"></a>Resumo

Um esquema de particionamento efetivo pode habilitar seu aplicativo de nuvem expandir a petabytes de dados na nuvem sem afunilamentos. E você não precise pagar antecipadamente para máquinas grandes ou infraestrutura ampla como se estivesse executando o aplicativo em um data center local. Na nuvem pode você pode adicionar paulatinamente capacidade conforme necessário, e você está pagando somente para o quanto você está usando quando você usá-lo.

No [próximo capítulo](unstructured-blob-storage.md) veremos como o aplicativo corrigir implementa o particionamento vertical ao armazenar imagens no armazenamento de Blob.

## <a name="resources"></a>Recursos

Para obter mais informações sobre estratégias de particionamento, consulte os seguintes recursos.

Documentação:

- [Práticas recomendadas para o Design de serviços em grande escala nos serviços de nuvem do Azure Windows](https://msdn.microsoft.com/en-us/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy.
- [Padrões e práticas - padrões de Design de nuvem Microsoft](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Consulte as diretrizes de particionamento de dados, o padrão de fragmentação.

Vídeos:

- [À prova de falhas: Criação de serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta os conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, com histórias extraídas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Consulte a discussão particionamento episódio 7.
- [Criando Big: Lições aprendidas de clientes do Windows Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms discute os esquemas de particionamento, estratégias de fragmentação, a implementação de fragmentação e federações de banco de dados SQL, iniciando em 19:49. Semelhante ao entrar em mais detalhes instruções mas série à prova de falhas.

Código de exemplo:

- [Conceitos fundamentais do serviço no Windows Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo que inclui um banco de dados fragmentado. Para obter uma descrição do esquema de fragmentação implementada, consulte [DAL – fragmentação de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) no blog do Windows Azure.

>[!div class="step-by-step"]
[Anterior](data-storage-options.md)
[Próximo](unstructured-blob-storage.md)
