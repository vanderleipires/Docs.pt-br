---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: (Criando aplicativos de nuvem do mundo Real com o Azure) de estratégias de particionamento de dados | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 1a19b5b8ce47439c39359a805e16ab4e7631deec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830819"
---
<a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>(Criando aplicativos de nuvem do mundo Real com o Azure) de estratégias de particionamento de dados
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre a série, consulte [o primeiro capítulo](introduction.md).


Anteriormente, vimos como é fácil dimensionar a camada da web de um aplicativo de nuvem, adicionando e removendo servidores web. Mas, se eles todos atingindo o mesmo armazenamento de dados, move de afunilamento do seu aplicativo de front-end para o back-end e a camada de dados é mais difíceis de dimensionar. Neste capítulo vamos ver como você pode fazer sua camada de dados escalonáveis por meio do particionamento de dados em vários bancos de dados relacionais ou pela combinação de armazenamento do banco de dados relacional com outras opções de armazenamento de dados.

Configurando um esquema de particionamento é melhor done com antecedência pela mesma razão, mencionado anteriormente: é muito difícil alterar sua estratégia de armazenamento de dados depois que um aplicativo está em produção. Se você acha difícil com antecedência sobre as diferentes abordagens, você pode evitar ter "Twitter instantes" quando seu aplicativo falhar ou fica inativo por um longo período enquanto você reorganizar dados e código de acesso a dados de seu aplicativo.

## <a name="the-three-vs-of-data-storage"></a>Os três Vs de armazenamento de dados

Para determinar se você precisa de uma estratégia de particionamento e o que deve ser, considere as três perguntas sobre seus dados:

- Volume – a quantidade de dados você vai, por fim, armazenar? Alguns gigabytes? Algumas centenas de gigabytes? Terabytes? Petabytes?
- Velocidade – o que é a taxa de crescem de seus dados? É um aplicativo interno que não está gerando muitos dados? Um aplicativo externo que os clientes serão fazendo o upload de imagens e vídeos em?
- Variedade – tipo de dados serão armazenados? Relacionais, imagens, pares chave-valor, gráficos sociais?

Se você achar que você vai ter muita de volume, velocidade ou vários, você precisa considerar cuidadosamente qual tipo de esquema de particionamento melhor permitirá que seu aplicativo para dimensionar com eficiência e eficácia à medida que ele cresce e para garantir que você não encontrar gargalos.

Há basicamente três abordagens para particionamento:

- Particionamento vertical
- Particionamento horizontal
- Particionamento híbrido

## <a name="vertical-partitioning"></a>Particionamento vertical

Portioning vertical é como dividir uma tabela por colunas: um conjunto de colunas entra em um repositório de dados, e outro conjunto de colunas entrará em um armazenamento de dados diferentes.

Por exemplo, suponha que meu aplicativo armazena dados sobre pessoas, incluindo as imagens:

![Tabela de dados](data-partitioning-strategies/_static/image1.png)

Quando você representa esses dados como uma tabela e examinar as diferentes variedades de dados, você pode ver que as três colunas à esquerda tem dados de cadeia de caracteres que podem ser armazenados com eficiência por um banco de dados relacional, enquanto as duas colunas à direita são essencialmente as matrizes de bytes que c ome de arquivos de imagem. É possível para o armazenamento de dados de arquivo de imagem em um banco de dados relacional, e muitas pessoas fazer isso porque não querem salvar os dados para o sistema de arquivos. Talvez não tenham um sistema de arquivos capaz de armazenar os volumes necessários de dados ou eles talvez não queira gerenciar um backup separado e restauração do sistema. Essa abordagem funciona bem para bancos de dados local e para pequenas quantidades de dados em bancos de dados de nuvem. No ambiente local, pode ser mais fácil deixar o DBA cuidar de tudo.

Mas em um banco de dados de nuvem, o armazenamento é relativamente caro, e um alto volume de imagens poderia fazer o tamanho do banco de dados crescer além dos limites em que ele pode operar com eficiência. Você pode resolver esses problemas por meio do particionamento de dados verticalmente, o que significa que você escolher o armazenamento de dados mais apropriado para cada coluna na tabela de dados. O que pode funcionar melhor para este exemplo é colocar os dados de cadeia de caracteres em um banco de dados relacional e as imagens no armazenamento de BLOBs.

![Tabela de dados particionada verticalmente](data-partitioning-strategies/_static/image2.png)

Armazenar imagens no armazenamento de Blob em vez de um banco de dados é mais prático na nuvem do que em um ambiente local, porque você não precisa se preocupar sobre como configurar servidores de arquivos ou gerenciamento de backup e restauração dos dados armazenados fora do banco de dados relacional: todos que é manipulado para você automaticamente pelo serviço de armazenamento de Blob.

Essa é a abordagem de particionamento que tenhamos implementado no aplicativo Fix It e vamos examinar o código para fazer isso na [capítulo de armazenamento de Blob](unstructured-blob-storage.md). Sem esse esquema de particionamento e supondo um tamanho de imagem média de 3 megabytes, o aplicativo Fix It só seria capaz de armazenar cerca de 40.000 tarefas antes de atingir o tamanho máximo do banco de dados de 150 gigabytes. Depois de remover as imagens, o banco de dados pode armazenar 10 vezes mais tarefas; Você pode ir muito mais tempo antes que você precisa pensar sobre como implementar um esquema de particionamento horizontal. E como o aplicativo é dimensionado, seus gastos crescem mais lentamente porque a maior parte de suas necessidades de armazenamento são entram muito barato armazenamento de BLOBs.

## <a name="horizontal-partitioning-sharding"></a>Particionamento horizontal (fragmentação)

Portioning horizontal é como dividir uma tabela por linhas: um conjunto de linhas entra em um repositório de dados, e outro conjunto de linhas entrará em um armazenamento de dados diferentes.

Dado o mesmo conjunto de dados, outra opção seria armazenar diferentes intervalos de nomes de clientes em diferentes bancos de dados.

![Tabela de dados particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Você deseja ter muito cuidado ao seu esquema de fragmentação para certificar-se de que os dados são distribuídos uniformemente para evitar pontos de acesso. Neste exemplo simples usando a primeira letra do sobrenome não atende a esse requisito, porque muitas pessoas têm os sobrenomes que começam com certas letras comuns. Atingia as limitações de tamanho de tabela anteriormente que o esperado porque alguns bancos de dados ficar muito grandes enquanto a maioria permanecerão pequena.

Um lado de particionamento horizontal é que talvez seja difícil fazer consultas em todos os dados. Neste exemplo, uma consulta precisaria desenhar de até 26 bancos de dados diferentes para obter todos os dados armazenados pelo aplicativo.

## <a name="hybrid-partitioning"></a>Particionamento híbrido

Você pode combinar o particionamento vertical e horizontal. Por exemplo, nos dados de exemplo, você pode armazenar as imagens no armazenamento de BLOBs e particionar horizontalmente os dados de cadeia de caracteres.

![Híbrido de tabela de dados particionado](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Particionamento de um aplicativo de produção

Conceitualmente, ele é fácil ver como funciona um esquema de particionamento, mas qualquer esquema de particionamento aumentar a complexidade do código e apresenta muitas complicações novo que você tem de lidar com. Se você estiver movendo imagens no armazenamento de BLOBs, o que fazer quando o serviço de armazenamento está inativo? Como você lida com segurança de blob? O que acontece se o armazenamento de BLOBs e banco de dados ficar fora de sincronizado? Se você estiver fragmentação, como você administrará consultando através de todos os bancos de dados?

As complicações são gerenciáveis, desde que você está planejando para eles antes de ir para a produção. Muitas pessoas que não fiz que gostariam de ter mais tarde. Em média nossa equipe de equipe de consultoria ao cliente (CAT) obtém em pânico chamadas telefônicas sobre uma vez por mês de clientes cujos aplicativos estão ficando muito populares de uma maneira realmente grande e não fazem esse planejamento. E eles dizem algo como: "Ajuda! Posso colocar tudo em um único armazenamento de dados e, em 45 dias, vou ficar sem espaço nele!" E se você tiver muita lógica de negócios integrada como você acessa o armazenamento de dados e você tiver clientes que estão usando seu aplicativo, não há nenhum tempo de BOM para ir para um dia durante a migração. Vamos percorrer os esforços de herculean para ajudar a partição de cliente seus dados em tempo real com nenhum tempo de inatividade. É muito empolgante e bastante assustador, e não algo que deseja estar envolvido no se você puder evitá-lo! Pensando nisso com antecedência e integrá-la em seu aplicativo fará sua vida muito mais fácil se o aplicativo crescer mais tarde.

## <a name="summary"></a>Resumo

Um esquema de particionamento eficiente pode habilitar seu aplicativo de nuvem dimensionar para petabytes de dados na nuvem sem gargalos. E você não precisa pagar antecipadamente por máquinas grandes ou uma infraestrutura extensiva assim como você faria se estivesse executando o aplicativo em um datacenter local. Na nuvem pode você pode adicionar incrementalmente a capacidade conforme necessário, e você está pagando somente para tanto quanto você está usando quando você usá-lo.

No [capítulo seguinte](unstructured-blob-storage.md) , veremos como o aplicativo Fix It implementa o particionamento vertical armazenando imagens no armazenamento de BLOBs.

## <a name="resources"></a>Recursos

Para obter mais informações sobre estratégias de particionamento, consulte os seguintes recursos.

Documentação:

- [Práticas recomendadas para o Design de serviços em larga escala nos serviços de nuvem do Azure Windows](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper, Mark Simms e Michael Thomassy.
- [Microsoft Patterns and Practices - padrões de Design de nuvem](https://msdn.microsoft.com/library/dn568099.aspx). Consulte as diretrizes de particionamento de dados, o padrão de fragmentação.

Vídeos:

- [FailSafe: Criação de serviços de nuvem escalonáveis, resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes por Marc Mercuri, Ulrich Homann e Mark Simms. Apresenta conceitos de alto nível e princípios de arquitetura de uma maneira muito acessível e interessante, histórias desenhadas da experiência do Microsoft Customer Advisory Team (CAT) com clientes reais. Consulte a discussão de particionamento episódio 7.
- [Grande de construção: Lições aprendidas de clientes do Windows Azure - parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms discute os esquemas de particionamento, estratégias de fragmentação, como implementar a fragmentação e federações de banco de dados SQL, começando em 19:49. Semelhante a série à prova de falhas, mas entra em mais detalhes de instruções.

Exemplo de código:

- [Conceitos fundamentais do serviço no Windows Azure na nuvem](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo que inclui um banco de dados fragmentado. Para obter uma descrição do esquema de fragmentação implementada, consulte [DAL – fragmentação de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) no blog do Windows Azure.

> [!div class="step-by-step"]
> [Anterior](data-storage-options.md)
> [Próximo](unstructured-blob-storage.md)
